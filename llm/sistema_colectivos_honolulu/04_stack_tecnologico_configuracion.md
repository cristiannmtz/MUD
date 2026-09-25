# 04 - Stack Tecnológico y Archivos de Configuración

Este documento detalla la configuración técnica exacta de build, librerías y archivos de propiedades que componen el proyecto de referencia.

---

## 📦 1. Dependencias en `pom.xml`

El archivo `pom.xml` de `sistema_colectivos_honolulu` utiliza las siguientes dependencias estándar:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>21</maven.compiler.source>
    <maven.compiler.target>21</maven.compiler.target>
    <javafx.version>21.0.2</javafx.version>
</properties>

<dependencies>
    <!-- Driver PostgreSQL JDBC -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <version>42.7.4</version>
    </dependency>
    
    <!-- JavaFX 21 -->
    <dependency>
        <groupId>org.openjfx</groupId>
        <artifactId>javafx-controls</artifactId>
        <version>${javafx.version}</version>
    </dependency>
    <dependency>
        <groupId>org.openjfx</groupId>
        <artifactId>javafx-fxml</artifactId>
        <version>${javafx.version}</version>
    </dependency>
    <dependency>
        <groupId>org.openjfx</groupId>
        <artifactId>javafx-graphics</artifactId>
        <version>${javafx.version}</version>
    </dependency>
    <dependency>
        <groupId>org.openjfx</groupId>
        <artifactId>javafx-web</artifactId>
        <version>${javafx.version}</version>
    </dependency>
    <dependency>
        <groupId>org.openjfx</groupId>
        <artifactId>javafx-media</artifactId>
        <version>${javafx.version}</version>
    </dependency>
    <dependency>
        <groupId>org.openjfx</groupId>
        <artifactId>javafx-swing</artifactId>
        <version>${javafx.version}</version>
    </dependency>

    <!-- Logging con Log4j2 -->
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-api</artifactId>
        <version>2.23.1</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.23.1</version>
    </dependency>

    <!-- Pruebas con JUnit 5 -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-engine</artifactId>
        <version>5.8.1</version>
    </dependency>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-api</artifactId>
        <version>5.8.1</version>
    </dependency>
</dependencies>
```

### Plugins de Maven Clave:
- `maven-compiler-plugin` (versión 3.11.0) para compilar en Java 21.
- `javafx-maven-plugin` (versión 0.0.8) apuntando a `colectivo.aplicacion.AplicacionPrincipal`.
- `maven-surefire-plugin` (versión 3.0.0) para ejecución de tests automatizados.

---

## ⚙️ 2. Archivos de Configuración (`.properties`)

### `factory.properties` (Ubicado en `src/main/resources/`)
Configura qué implementación de DAO se cargará en tiempo de ejecución:
```properties
# Implementación SECUENCIAL (archivos de texto)
PARADA=colectivo.dao.secuencial.ParadaDAOSecuencial
LINEA=colectivo.dao.secuencial.LineaDAOSecuencial
TRAMO=colectivo.dao.secuencial.TramoDAOSecuencial

# Implementación SQL (base de datos)
#PARADA=colectivo.dao.BD.ParadaDAOBD
#LINEA=colectivo.dao.BD.LineaDAODB
#TRAMO=colectivo.dao.BD.TramoDAODB
```

### `jdbc.properties` (Ubicado en la raíz del proyecto)
Configura los parámetros de conexión JDBC para PostgreSQL:
```properties
usr=estudiante
pwd=estudiante
driver=org.postgresql.Driver
schema=colectivo_PM
url=jdbc:postgresql://pgs.fi.mdn.unp.edu.ar:30000/bd1
```

### `secuencial.properties` (Ubicado en `src/main/resources/`)
Define los nombres de los archivos `.txt` para almacenamiento plano:
```properties
parada=parada_PM.txt
linea=linea_PM.txt
tramo=tramo_PM.txt
frecuencia=frecuencia_PM.txt
```

---

## 📝 3. Configuración de Logging (`log4j2.xml`)

Ubicado en `src/main/resources/log4j2.xml`:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
        <File name="File" fileName="colectivo-app.log">
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </File>
    </Appenders>
    <Loggers>
        <Root level="WARN">
            <AppenderRef ref="Console"/>
            <AppenderRef ref="File"/>
        </Root>
    </Loggers>
</Configuration>
```
Este esquema permite ver logs limpios en consola y mantener una traza completa persistida en archivo `.log`.
