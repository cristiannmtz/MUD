# Reglas de Desarrollo del Proyecto MUD

Este compendio establece las reglas obligatorias e inquebrantables que todo desarrollador y asistente LLM debe seguir durante la construcción del proyecto **MUD**.

---

## 📑 Índice de Reglas

1. [01. Arquitectura MVC y Capas](file:///g:/Universidad/2026/POO/ISFPP/MUD/llm/reglas/01_arquitectura_mvc.md)
   - Estructura de paquetes canónica.
   - Responsabilidad única de cada capa.
   - Reglas de acoplamiento y prohibiciones de importación.

2. [02. Tecnologías Permitidas y Prohibiciones](file:///g:/Universidad/2026/POO/ISFPP/MUD/llm/reglas/02_tecnologias_permitidas.md)
   - Lista cerrada de tecnologías autorizadas.
   - Principio: *"No podés usar otras cosas más complicadas"*.
   - Prohibición de Spring, Hibernate, JPA, Lombok, etc.

3. [03. Patrones de Diseño Canónicos](file:///g:/Universidad/2026/POO/ISFPP/MUD/llm/reglas/03_patrones_diseno.md)
   - Factory dinámico con reflexión y caché singleton.
   - DAO genérico y persistencia dual (Secuencial y PostgreSQL).
   - Coordinador de aplicación (Application Manager) y puente estático.
   - Interfaz de Vista para desacoplar JavaFX o Consola.

4. [04. Directrices para el Asistente LLM y Bitácora](file:///g:/Universidad/2026/POO/ISFPP/MUD/llm/reglas/04_directrices_llm_bitacora.md)
   - Protocolo obligatorio de actualización de la bitácora tras cada cambio.
   - Verificación de consistencia y apego a las convenciones de la cátedra.
