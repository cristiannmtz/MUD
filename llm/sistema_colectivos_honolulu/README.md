# Documentación de Referencia: `sistema_colectivos_honolulu`

Este módulo documenta de forma exhaustiva la arquitectura, decisiones de diseño, patrones y tecnologías utilizadas en el proyecto de referencia ubicado en:
`G:\Universidad\2026\New folder\sistema_colectivos_honolulu`

---

## 📚 Índice de Documentación

1. [01. Análisis de Arquitectura y Estructura](file:///g:/Universidad/2026/POO/ISFPP/MUD/llm/sistema_colectivos_honolulu/01_analisis_arquitectura.md)
   - Desglose paquete por paquete.
   - Flujo de control desde `AplicacionPrincipal` hasta la interfaz.
   - Diagrama de clases y relaciones de capas.

2. [02. Patrón Factory y Persistencia Dual (DAO)](file:///g:/Universidad/2026/POO/ISFPP/MUD/llm/sistema_colectivos_honolulu/02_patron_factory_dao.md)
   - Explicación de `Factory.java` con reflexión y `ResourceBundle`.
   - Implementación de `DAO<K, V>`.
   - Comparación de `*DAOSecuencial` (archivos `.txt` con separador `;`) y `*DAOBD` (PostgreSQL JDBC).
   - Cambio de persistencia en caliente mediante `factory.properties`.

3. [03. Coordinador de Aplicación, Servicios y UI](file:///g:/Universidad/2026/POO/ISFPP/MUD/llm/sistema_colectivos_honolulu/03_coordinador_servicios_ui.md)
   - Rol y código clave de `CoordinadorApp`.
   - Capa de servicios (`*Service` y `*ServiceImpl`) para validaciones.
   - Contrato de desacoplamiento `Interfaz` y su implementación en JavaFX (`InterfazJavaFXImpl`).

4. [04. Stack Tecnológico y Archivos de Configuración](file:///g:/Universidad/2026/POO/ISFPP/MUD/llm/sistema_colectivos_honolulu/04_stack_tecnologico_configuracion.md)
   - Desglose de `pom.xml` (Java 21, JavaFX 21.0.2, PostgreSQL 42.7.4, Log4j2 2.23.1, JUnit 5.8.1).
   - Archivos `.properties` (`factory.properties`, `jdbc.properties`, `secuencial.properties`).
   - Configuración de logging en `log4j2.xml`.
