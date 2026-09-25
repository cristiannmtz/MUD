# [0002] - Esqueleto Parte 1: Enumeraciones y Entidades Base de Soporte

- **Fecha:** 2026-09-25 15:15 (America/Argentina/Buenos_Aires)
- **Solicitud del Usuario:** "Tenemos que hacer el siguiente diagrama, hazlo por partes, solo el esqueleto, no implementes metodos"
- **Estado:** Completado (Parte 1 de 4)

---

### 1. Objetivo
Iniciar la construcción del esqueleto del diagrama de clases UML proporcionado por el usuario, dividiendo la tarea en 4 partes bien definidas y manteniendo únicamente firmas y atributos sin implementar lógica interna de métodos.

### 2. Archivos Afectados
- `pom.xml` (Creación - Configuración Maven idéntica a la referencia con Java 21)
- `src/main/java/mud/modelo/TipoServicio.java` (Creación - Enum: AUTO, MOTO)
- `src/main/java/mud/modelo/Estado.java` (Creación - Enum de estados de viaje)
- `src/main/java/mud/modelo/Calificacion.java` (Creación - Enum de niveles de calificación)
- `src/main/java/mud/modelo/Ubicacion.java` (Creación - Entidad con latitud, longitud, dirección)
- `src/main/java/mud/modelo/Servicio.java` (Creación - Entidad con tarifas y TipoServicio)

### 3. Descripción Detallada de Cambios
1. **Planificación en 4 partes del diagrama UML**:
   - **Parte 1 (Esta entrega)**: Enumeraciones (`TipoServicio`, `Estado`, `Calificacion`) y entidades de soporte (`Ubicacion`, `Servicio`).
   - **Parte 2**: Jerarquía de Vehículos (`Vehiculo`, `Auto`, `Moto`).
   - **Parte 3**: Jerarquía de Usuarios (`Usuario`, `Conductor`, `Cliente`).
   - **Parte 4**: Entidad orquestadora central (`Viaje`).
2. **Generación del código esqueleto de la Parte 1**:
   - Se crearon los enums y clases POJO en el paquete `mud.modelo`.
   - Se definieron atributos privados, constructores vacíos y parametrizados, getters, setters y `toString()`. No se agregaron métodos de negocio ni lógica externa.

### 4. Decisiones de Diseño y Alineación con Reglas
- Paquete canónico `mud.modelo` correspondiente a la capa de dominio.
- Ninguna dependencia externa compleja agregada; solo Java 21 estándar.

### 5. Verificación y Pruebas
- Compilación exitosa con `javac 21.0.12.1` hacia `target/classes` sin errores ni advertencias.

### 6. Próximos Pasos
- Avanzar con la **Parte 2: Jerarquía de Vehículos** (`Vehiculo`, `Auto`, `Moto`), vinculando sus relaciones con `Servicio` y `Ubicacion`.
