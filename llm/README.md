# Estructura de Gobierno y Desarrollo con LLM

Esta carpeta (`llm/`) centraliza el marco de trabajo, reglas de desarrollo, bitácora de cambios y el análisis del proyecto de referencia **`sistema_colectivos_honolulu`** para el proyecto **MUD**.

---

## 📁 Estructura del Directorio

```
llm/
├── README.md                           # Visión general y guía del directorio
├── bitacora/                           # Registro histórico de cada cambio realizado
│   ├── README.md                       # Protocolo y plantilla de registro en bitácora
│   └── 0001_inicializacion_estructura_referencia.md
├── reglas/                             # Reglas estrictas de arquitectura, stack y código
│   ├── README.md                       # Índice de reglas y mandatos principales
│   ├── 01_arquitectura_mvc.md          # Estructura en capas, desacoplamiento y flujo
│   ├── 02_tecnologias_permitidas.md    # Stack técnico autorizado y tecnologías prohibidas
│   ├── 03_patrones_diseno.md           # Patrones adoptados (Factory, DAO, Coordinador, etc.)
│   └── 04_directrices_llm_bitacora.md  # Obligaciones del LLM para cada tarea
└── sistema_colectivos_honolulu/        # Documentación y análisis del proyecto de referencia
    ├── README.md                       # Resumen del proyecto de referencia
    ├── 01_analisis_arquitectura.md     # Estructura de paquetes y responsabilidades
    ├── 02_patron_factory_dao.md        # Implementación de Factory y DAOs duales (BD y Secuencial)
    ├── 03_coordinador_servicios_ui.md  # Desacoplamiento de Vista y rol del Coordinador
    └── 04_stack_tecnologico_configuracion.md # pom.xml, configs (.properties) y logging
```

---

## 🎯 Mandato Principal

1. **Simplicidad ante todo ("no usar cosas más complicadas")**:
   - Se debe replicar el modelo exacto de `sistema_colectivos_honolulu`.
   - Prohibido el uso de frameworks pesados (Spring Boot, Hibernate, JPA, Lombok, bases NoSQL, etc.).
   - Utilizar únicamente **Java 21 estándar, JavaFX, JDBC PostgreSQL, persistencia secuencial en archivos `.txt`, Log4j2, JUnit 5 y configuración con `.properties`**.

2. **MVC / Arquitectura por Capas con Coordinador**:
   - `modelo`: Entidades POJO puras.
   - `dao`: Interfaces genéricas y persistencia dual (`dao.secuencial` y `dao.BD`).
   - `conexion`: `Factory` basado en reflexión + `ResourceBundle` y `BDConexion` nativa JDBC.
   - `servicio`: Lógica de servicio y validaciones (`*Service` y `*ServiceImpl`).
   - `negocio`: Algoritmos y cálculos independientes.
   - `controlador`: `CoordinadorApp` puro, sin dependencias de GUI.
   - `ui`: Interfaz desacoplada (`Interfaz`) e implementaciones (JavaFX o Consola).
   - `aplicacion`: `AplicacionPrincipal` como punto de entrada puro.

3. **Bitácora Obligatoria**:
   - **CADA cambio**, adición o refactorización debe registrarse inmediatamente en `llm/bitacora/` siguiendo la plantilla definida.
