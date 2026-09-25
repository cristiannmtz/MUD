# 03 - Reglas de Patrones de Diseño Canónicos

Los patrones aplicados en `sistema_colectivos_honolulu` son la base obligatoria para el diseño del proyecto **MUD**. No se deben inventar patrones complejos no justificados.

---

## 1. Patrón Factory con Reflexión y Singleton Cache (`Factory.java`)

### Propósito
Permitir el intercambio transparente entre implementaciones de persistencia (Base de Datos PostgreSQL vs. Archivos Secuenciales de Texto) sin alterar el código de los servicios ni del controlador.

### Reglas de Implementación
- La clase `Factory` es `final` con constructor privado para evitar instanciación.
- Lee las clases a instanciar desde `factory.properties` usando `ResourceBundle.getBundle("factory")`.
- Almacena las instancias creadas en un `ConcurrentHashMap<String, Object>` estático para garantizar que se comporten como singletons thread-safe.
- Proporciona un método genérico con verificación de tipos en tiempo de ejecución:
  ```java
  public static <T> T getInstancia(String objName, Class<T> expectedType)
  ```
- Lanza `ClassCastException` si la clase configurada no coincide con el tipo esperado.

---

## 2. Patrón Data Access Object (DAO) Dual

### Propósito
Aislar la capa de persistencia mediante una interfaz genérica común.

### Reglas de Implementación
- Toda entidad persistible debe contar con una interfaz específica que extienda de `DAO<K, V>`:
  ```java
  public interface DAO<K, V> {
      void insertar(V value);
      void actualizar(V value);
      void borrar(V value);
      Map<K, V> buscarTodos();
  }
  ```
- Debe existir siempre una implementación en `dao.secuencial.*DAOSecuencial` (I/O con archivos `.txt`, usando separador `;` y `BufferedReader`/`BufferedWriter`).
- Debe existir siempre una implementación en `dao.BD.*DAOBD` (conectividad JDBC con `BDConexion`, `PreparedStatement` y `ResultSet`).
- Ningún DAO debe conocer la existencia de otro DAO ni invocar lógica de negocio.

---

## 3. Patrón Coordinador (Application Coordinator / Manager)

### Propósito
Centralizar el ensamblaje de la aplicación, actuar como puente entre la interfaz de usuario y los servicios, y mantener al controlador libre de dependencias de tecnologías de GUI.

### Reglas de Implementación
- La clase `CoordinadorApp`:
  1. Instancia los DAOs a través de `Factory.getInstancia(...)`.
  2. Inyecta los DAOs a las implementaciones de los servicios correspondientes (`*ServiceImpl`).
  3. Carga opcionalmente datos maestros en memoria si la aplicación lo requiere.
  4. Crea la vista (`Interfaz`) e inyecta la referencia al coordinador mediante `interfaz.setCoordinador(this)`.
  5. Inicia la aplicación invocando `interfaz.iniciar()`.
- **Cero dependencias gráficas**: El coordinador no debe importar `javafx.*` ni ningún componente visual.

---

## 4. Patrón Interfaz de Vista (View Contract)

### Propósito
Permitir que la interfaz pueda ejecutarse tanto en JavaFX (gráfico) como en consola (terminal) o Swing sin modificar el Coordinador.

### Reglas de Implementación
- Se define una interfaz `Interfaz` con:
  ```java
  public interface Interfaz {
      void setCoordinador(CoordinadorApp coordinador);
      void iniciar();
  }
  ```
- La clase `InterfazJavaFXImpl` o `InterfazConsolaImpl` implementa este contrato.

---

## 5. Patrón Strategy (Estrategia)

### Propósito
Encapsular algoritmos variables (por ejemplo, cálculo de caminos, algoritmos de combate o reglas de movimiento en MUD) en clases independientes que implementan una interfaz común.

---

## 6. Patrón Singleton

### Propósito
Garantizar una única instancia compartida para recursos globales como:
- Conexión JDBC (`BDConexion`).
- Administrador de mensajes e internacionalización (`MessageManager`).
- Caché de instancias en `Factory`.
