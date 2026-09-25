# Bitácora de Cambios del Proyecto MUD

Este directorio contiene el registro cronológico y exhaustivo de todos los cambios, decisiones de diseño, implementaciones y refactorizaciones realizadas en el proyecto con la asistencia del LLM.

---

## 📌 Protocolo de Registro

1. **Obligatoriedad**: Cada interacción o tarea que modifique, cree o elimine archivos debe generar o actualizar un archivo en esta carpeta.
2. **Nomenclatura**:
   - Formato: `XXXX_descripcion_corta.md` (ejemplo: `0001_inicializacion_estructura_referencia.md`, `0002_modelo_dominio_habitacion.md`).
   - El número de cuatro dígitos debe ser estrictamente incremental.
3. **Contenido Mínimo de Cada Entrada**:
   - **Metadatos**: Fecha y hora local, tarea solicitada, autor/asistente.
   - **Objetivo**: Qué se buscaba lograr.
   - **Archivos Modificados / Creados**: Lista explícita de rutas relativas.
   - **Decisiones Técnicas / Justificación**: Por qué se implementó de esa forma y cómo se alinea con las reglas.
   - **Validación / Pruebas**: Cómo se verificó el cambio.
   - **Estado y Próximos Pasos**: Estado final de la tarea y qué queda pendiente.

---

## 📋 Plantilla para Nuevas Entradas

```markdown
# [XXXX] - Título Breve del Cambio

- **Fecha:** AAAA-MM-DD HH:MM (Zona horaria)
- **Solicitud del Usuario:** "<Texto o resumen de la petición>"
- **Estado:** [Completado / En Progreso / Pendiente]

---

### 1. Objetivo
Descripción clara del propósito del cambio.

### 2. Archivos Afectados
- \`archivo_creado.java\` (Creación)
- \`archivo_modificado.java\` (Modificación)

### 3. Descripción Detallada de Cambios
Explicación técnica de lo realizado paso a paso.

### 4. Decisiones de Diseño y Alineación con Reglas
- Justificación basada en el proyecto de referencia (\`sistema_colectivos_honolulu\`).
- Confirmación de tecnologías permitidas utilizadas.

### 5. Verificación y Pruebas
- Comandos ejecutados, compilación, tests unitarios.

### 6. Próximos Pasos
- Siguiente tarea pendiente.
```
