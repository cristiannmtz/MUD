# 04 - Directrices Obligatorias para el Asistente LLM y Bitácora

Este documento establece las instrucciones operativas que el asistente LLM **debe** seguir en cada turno de conversación o tarea de desarrollo en este repositorio.

---

## 🤖 Protocolo Obligatorio del Asistente LLM

1. **Consulta Previa de Reglas:**
   Antes de generar cualquier clase, script o archivo de configuración, el LLM debe contrastar la propuesta contra `llm/reglas/01_arquitectura_mvc.md` y `llm/reglas/02_tecnologias_permitidas.md`.

2. **Registro Inmediato en Bitácora:**
   - **En cada turno donde se creen, modifiquen o eliminen archivos de código o configuración, el LLM debe generar o actualizar una entrada en `llm/bitacora/`**.
   - No finalizar la respuesta al usuario sin haber actualizado la bitácora correspondiente.
   - Usar la numeración incremental continua (`0001_...`, `0002_...`, etc.).

3. **Restricción de Simplicidad Absoluta:**
   - Si una tarea se puede resolver con POO pura, colecciones de Java (`List`, `Map`, `Set`), JavaFX básico y JDBC/archivos de texto, **no** agregar ninguna librería adicional.
   - Prohibido sugerir frameworks externos salvo que el usuario lo solicite expresamente y se confirme la excepción.

4. **Preservación de Convenciones:**
   - Código en castellano o inglés consistente con la cátedra (en `sistema_colectivos_honolulu` los paquetes y clases de dominio están en castellano: `aplicacion`, `conexion`, `controlador`, `dao`, `modelo`, `servicio`, `ui`, `util`).
   - Todos los mensajes de log deben usar `Log4j2` (`LogManager.getLogger(...)`).
   - Toda configuración externa sensible debe residir en archivos `.properties` en la raíz o en `src/main/resources`.

5. **Transparencia en la Respuesta al Usuario:**
   - Al responder al usuario, resumir las acciones realizadas y hacer enlaces a los archivos creados o actualizados mediante enlaces de Markdown en formato `[nombre_archivo](file:///ruta/al/archivo)`.
