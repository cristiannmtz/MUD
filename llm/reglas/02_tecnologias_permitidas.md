# 02 - Reglas de Tecnologías Permitidas

> **Regla de Oro:** *"No podés usar otras cosas más complicadas"*.  
> El proyecto debe usar única y exclusivamente las mismas herramientas, versiones y librerías encontradas en el proyecto de referencia `sistema_colectivos_honolulu`.

---

## ✅ Tecnologías y Librerías Autorizadas

| Componente | Tecnología / Librería | Versión / Detalle | Justificación |
| :--- | :--- | :--- | :--- |
| **Lenguaje** | Java SE | **Java 21** | Estándar de la cátedra universitaria. |
| **Gestor de Construcción**| Apache Maven | `pom.xml` | Mismo esquema de plugins que la referencia. |
| **Interfaz de Usuario** | OpenJFX (JavaFX) | `21.0.2` (`controls`, `fxml`, `graphics`, `web`, `media`, `swing`) | Soporte para interfaces gráficas modernas y desacopladas. También se permite interfaz de consola por terminal. |
| **Driver de Base de Datos**| PostgreSQL JDBC | `42.7.4` (`org.postgresql:postgresql`) | Conectividad estándar mediante JDBC puro (`Connection`, `PreparedStatement`, `ResultSet`). |
| **Persistencia Alternativa**| Archivos de Texto Plano | Estándar Java I/O (`BufferedReader`, `BufferedWriter`, delimitado por `;`) | Persistencia secuencial en archivos `.txt` configurable vía `.properties`. |
| **Logging** | Apache Log4j2 | `2.23.1` (`log4j-api`, `log4j-core`) | Logging estructurado mediante `log4j2.xml`. |
| **Testing** | JUnit Jupiter | `5.8.1` (`junit-jupiter-api`, `junit-jupiter-engine`) | Pruebas unitarias de servicios, algoritmos y DAOs. |
| **Configuración & i18n** | Java Standard | `ResourceBundle` y `Properties` (`.properties`) | Configuración dinámica de factoría (`factory.properties`), conexión (`jdbc.properties`) y traducción de textos. |

---

## ❌ Tecnologías y Herramientas Terminantemente Prohibidas

1. **Frameworks empresariales pesados:**
   - 🚫 **Spring / Spring Boot**: Prohibido. La inyección de dependencias es manual o vía la clase `Factory` por reflexión.
   - 🚫 **Jakarta EE / Quarkus / Micronaut**: Prohibido.
2. **ORMs y capas de abstracción de datos complejas:**
   - 🚫 **Hibernate / JPA**: Prohibido. Todas las consultas SQL deben escribirse a mano con `PreparedStatement`.
   - 🚫 **MyBatis / JOOQ / Spring Data**: Prohibido.
3. **Generadores de código y metaprogramación compleja:**
   - 🚫 **Project Lombok** (`@Getter`, `@Setter`, `@Builder`, etc.): Prohibido. Los POJOs deben tener código Java explícito (constructores, getters, setters tradicionales).
4. **Bases de datos no convencionales:**
   - 🚫 **MongoDB, Redis, SQLite (salvo autorización), bases NoSQL**: Prohibido. Sólo PostgreSQL vía JDBC y archivos `.txt` secuenciales.
5. **Herramientas de construcción no consensuadas:**
   - 🚫 **Gradle**: Prohibido salvo requerimiento explícito. Mantener Maven (`pom.xml`).
6. **Librerías gráficas invasivas o reactivas:**
   - 🚫 **JavaFX UI frameworks pesados** (tipo AtlantaFX, ScenicView, etc., salvo componentes JavaFX estándar).

---

## 📌 Principio de Diseño POO Universitario
El código debe reflejar los fundamentos de Programación Orientada a Objetos:
- Encapsulamiento riguroso.
- Herencia bien justificada.
- Polimorfismo mediante interfaces.
- Delegación de responsabilidades.
- Legibilidad y transparencia sin "magia" de frameworks opacos.
