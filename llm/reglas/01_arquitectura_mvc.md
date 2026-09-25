# 01 - Reglas de Arquitectura MVC y Capas

La arquitectura del proyecto debe seguir estrictamente el patrón por capas observado en `sistema_colectivos_honolulu`. Es una variante de MVC / MVP desacoplado mediante el patrón **Coordinador**.

---

## 🏛️ Estructura Canónica de Paquetes

```
mud/
├── aplicacion/          # Punto de entrada y constantes globales
├── conexion/            # Conexión JDBC (BDConexion) y Factory por reflexión
├── controlador/         # Coordinador central de la aplicación (CoordinadorApp)
├── dao/                 # Interfaces DAO y submódulos de persistencia
│   ├── BD/              # Implementaciones con JDBC nativo (PostgreSQL)
│   └── secuencial/      # Implementaciones con archivos de texto plano (.txt)
├── modelo/              # Clases de dominio puras (POJOs)
├── negocio/             # Algoritmos complejos de juego, cálculos, estrategias
├── servicio/            # Capa de servicios e interfaces (Lógica de aplicación)
├── ui/                  # Vistas, presentadores y componentes de interfaz
│   └── componentes/     # Controles y vistas personalizadas
└── util/                # Internacionalización (MessageManager) y utilidades
```

---

## 📐 Responsabilidad por Capa

| Capa | Responsabilidad | Permitido | Prohibido |
| :--- | :--- | :--- | :--- |
| **`modelo`** | Entidades de dominio (ej. `Jugador`, `Habitacion`, `Item`). | Atributos, getters, setters, constructores, `equals`, `hashCode`, `toString`, validaciones intrínsecas. | Importar clases de base de datos, JDBC, JavaFX, o DAOs. |
| **`dao`** | Acceso y persistencia de datos. | Heredar de `DAO<K, V>`, operaciones CRUD, manejo de SQL o I/O de archivos. | Contener lógica de negocio, cálculos de juego o llamadas a la UI. |
| **`conexion`** | Infraestructura de conexión y factoría. | `BDConexion` (singleton JDBC), `Factory` (reflexión con `ConcurrentHashMap`). | Mezclar lógica de negocio. |
| **`servicio`** | Fachada de casos de uso y validación previa a persistencia. | Inyectar DAOs, validar reglas previas, retornar DTOs o entidades. | Manipular componentes visuales (JavaFX) o ejecutar SQL directamente. |
| **`negocio`** | Algoritmos de dominio (combate, recorridos, parsing de comandos MUD). | Clases puras de lógica, patrón Strategy. | Acceso directo a base de datos o a la interfaz gráfica. |
| **`controlador`** | `CoordinadorApp`: "El Gerente" que orquesta el sistema. | Ensamblar servicios, inicializar caché en memoria, proveer datos a la UI. | Importar paquetes de `javafx.*` o Swing. Debe ser 100% Java puro. |
| **`ui`** | Renderizado e interacción con el usuario. | Implementar `Interfaz`, JavaFX / Swing / Consola, Presentadores. | Acceder directamente a DAOs o hacer conexiones a la BD. Todo pasa por el Coordinador. |
| **`aplicacion`**| Arranque del sistema. | `main()` que instancia `CoordinadorApp` y llama a `iniciarAplicacion()`. | Lógica de negocio o UI compleja en el `main()`. |

---

## 🚫 Reglas de Dependencia y Acoplamiento

1. **La UI nunca habla con los DAOs ni con la Base de Datos:**
   Toda petición visual (listar objetos, mover personaje, consultar estado) se solicita a través del `CoordinadorApp` o de un presentador que delega en el Coordinador.
2. **El Coordinador no conoce la tecnología de UI:**
   `CoordinadorApp` sólo conoce el contrato de la interfaz `Interfaz` (`void setCoordinador(...)`, `void iniciar()`). No debe haber ningún `import javafx...` en `coordinador`.
3. **Punto de Entrada Puro (Static Bridge):**
   `AplicacionPrincipal` contiene `public static void main(String[] args)` sin extender `javafx.application.Application`. La invocación de JavaFX se encapsula dentro de la capa `ui`.
4. **Persistencia Transparente:**
   Los servicios sólo conocen la interfaz DAO (`ParadaDAO`, `HabitacionDAO`). No saben si detrás hay un archivo `.txt` o una base PostgreSQL. La decisión se toma dinámicamente mediante `Factory.getInstancia(...)` leyendo `factory.properties`.
