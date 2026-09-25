# 02 - Patrón Factory y Persistencia Dual (DAO)

Uno de los aspectos más destacados de `sistema_colectivos_honolulu` es cómo desacopla la persistencia de datos mediante el patrón **Factory con reflexión** y la abstracción **DAO**.

---

## 🏭 1. La Factoría Dinámica (`Factory.java`)

En lugar de usar frameworks de inyección de dependencias (como Spring `@Autowired`), el proyecto implementa un Service Locator / Factory dinámico ligero y seguro:

```java
public final class Factory {
    private static final ConcurrentHashMap<String, Object> INSTANCIAS = new ConcurrentHashMap<>();
    private static final Logger LOG = LogManager.getLogger(Factory.class);

    private Factory() {
        throw new AssertionError("No instanciable");
    }

    @SuppressWarnings("unchecked")
    public static <T> T getInstancia(String objName, Class<T> expectedType) {
        Object instance = INSTANCIAS.computeIfAbsent(objName, Factory::crearInstancia);

        if (!expectedType.isInstance(instance)) {
            String error = String.format("ERROR: %s Esperado: %s, Obtenido: %s",
                    objName, expectedType.getName(), instance.getClass().getName());
            LOG.error(error);
            throw new ClassCastException(error);
        }
        return (T) instance;
    }

    private static Object crearInstancia(String clave) {
        try {
            ResourceBundle rb = ResourceBundle.getBundle("factory");
            if (!rb.containsKey(clave)) {
                throw new IllegalArgumentException("Clave no encontrada en factory.properties: " + clave);
            }
            String className = rb.getString(clave);
            return Class.forName(className).getDeclaredConstructor().newInstance();
        } catch (Exception ex) {
            throw new RuntimeException("Error Factory al crear: " + clave, ex);
        }
    }
}
```

### Configuración en `factory.properties`:
```properties
# Implementación SECUENCIAL (archivos de texto)
PARADA=colectivo.dao.secuencial.ParadaDAOSecuencial
LINEA=colectivo.dao.secuencial.LineaDAOSecuencial
TRAMO=colectivo.dao.secuencial.TramoDAOSecuencial

# Implementación SQL (base de datos PostgreSQL)
#PARADA=colectivo.dao.BD.ParadaDAOBD
#LINEA=colectivo.dao.BD.LineaDAODB
#TRAMO=colectivo.dao.BD.TramoDAODB
```

---

## 🗄️ 2. El Contrato Genérico DAO (`DAO.java`)

```java
public interface DAO<K, V> {
    public void insertar(V value);   
    public void actualizar(V value);   
    public void borrar(V value);   
    public Map<K, V> buscarTodos();
}
```
Y su especialización por entidad:
```java
public interface ParadaDAO extends DAO<Integer, Parada> { }
```

---

## 📁 3. Persistencia Secuencial (`*DAOSecuencial.java`)

- Lee la ruta del archivo de texto desde `secuencial.properties` (ej: `parada=parada_PM.txt`).
- Utiliza formato plano delimitado por punto y coma (`;`):
  `codigo;direccion;latitud;longitud;`
- **Inserción**: `BufferedWriter` con `FileWriter(archivo, true)` (append).
- **Actualización**: Reescribe el archivo completo iterando sobre el mapa de entidades.
- **Lectura**: `BufferedReader` con `readLine()`, separando campos con `linea.split(";")`.

---

## 🐘 4. Persistencia en Base de Datos (`*DAOBD.java` y `BDConexion.java`)

- `BDConexion` carga `jdbc.properties` al inicio:
  ```properties
  usr=estudiante
  pwd=estudiante
  driver=org.postgresql.Driver
  schema=colectivo_PM
  url=jdbc:postgresql://pgs.fi.mdn.unp.edu.ar:30000/bd1
  ```
- Gestiona la conexión mediante `DriverManager.getConnection(...)` y registra un `ShutdownHook` para cerrar la conexión limpiamente al apagar la JVM.
- `ParadaDAOBD` utiliza **`PreparedStatement`** nativo con parámetros parametrizados (`?`) para evitar inyección SQL:
  ```java
  String sql = "INSERT INTO parada (codigo, direccion, latitud, longitud) VALUES (?, ?, ?, ?)";
  try (Connection conn = BDConexion.getConnection();
       PreparedStatement pstmt = conn.prepareStatement(sql)) {
      pstmt.setString(1, parada.getCodigo());
      pstmt.setString(2, parada.getDireccion());
      pstmt.setDouble(3, parada.getLatitud());
      pstmt.setDouble(4, parada.getLongitud());
      pstmt.executeUpdate();
  }
  ```

---

## ⚖️ Conclusión para el Proyecto MUD
El proyecto MUD debe implementar este mismo esquema:
- Una interfaz `HabitacionDAO`, `JugadorDAO`, `ItemDAO`.
- Implementaciones `HabitacionDAOSecuencial` (usando archivos `.txt`) y `HabitacionDAOBD` (PostgreSQL).
- Una clase `Factory` idéntica configurada mediante `factory.properties`.
