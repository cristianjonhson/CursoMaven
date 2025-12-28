# 🚀 Configuración de Pipeline para CursoMaven/Arquetipos/gh

Este documento explica cómo configurar el pipeline para el proyecto específico del repositorio CursoMaven.

## 📋 Análisis del Proyecto

### Información del POM.xml

```xml
<groupId>gh</groupId>
<artifactId>gh</artifactId>
<version>1.0-SNAPSHOT</version>

<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <java-chassis.version>2.1.2.RELEASE</java-chassis.version>
</properties>
```

### Características del Proyecto

- **Framework**: Apache ServiceComb (Java-Chassis 2.1.2)
- **Versión Java**: Java 8 (configurado en maven-compiler-plugin)
- **Main Class**: `gh.Application`
- **Tipo**: Aplicación de microservicios REST

### Dependencias Principales

- `transport-rest-vertx` - Transporte REST con Vert.x
- `transport-highway` - Transporte de alto rendimiento
- `provider-springmvc` - Provider estilo Spring MVC
- `log4j` - Logging

### Plugins Maven Configurados

1. **maven-compiler-plugin** (v3.1)
   - Source: Java 8
   - Target: Java 8

2. **maven-jar-plugin** (v2.6)
   - Crea JAR ejecutable
   - Main Class: gh.Application
   - Classpath configurado para lib/

3. **maven-dependency-plugin**
   - Copia dependencias a `target/lib/`
   - Facilita despliegue con todas las librerías

## 🔧 Configuración en Jenkins

### 1. Crear el Job

1. En Jenkins, haz clic en **"New Item"**
2. Nombre: `CursoMaven-Arquetipo-GH`
3. Tipo: **Pipeline**
4. Click **OK**

### 2. Configurar el Pipeline

#### Opción A: Pipeline desde SCM (Recomendado)

1. En **Pipeline** → **Definition**: Selecciona "Pipeline script from SCM"
2. **SCM**: Git
3. **Repository URL**: `https://github.com/cristianjonhson/CursoMaven.git`
4. **Branch**: `*/master`
5. **Script Path**: `Jenkinsfile` (debes crear este archivo en la raíz del repo)

#### Opción B: Pipeline Script Directo

1. En **Pipeline** → **Definition**: Selecciona "Pipeline script"
2. Copia el contenido de `examples/Jenkinsfile-CursoMaven-gh`
3. Pega en el campo **Script**

### 3. Configurar Herramientas

El pipeline usa estas herramientas (ya preconfiguradas en el contenedor):

- **JDK**: `Java-8` (requerido por el proyecto)
- **Maven**: `Maven-3.6` (compatible con Java 8)

## 🎯 Etapas del Pipeline

### 1. Información del Entorno (📋)
```groovy
stage('📋 Información del Entorno')
```
- Muestra versiones de Java y Maven
- Verifica configuración correcta

### 2. Checkout (📥)
```groovy
stage('📥 Checkout')
```
- Clona el repositorio desde GitHub
- Branch: master

### 3. Clean (🧹)
```groovy
stage('🧹 Clean')
```
- Ejecuta: `mvn clean`
- Limpia artifacts de builds anteriores

### 4. Validar (✅)
```groovy
stage('✅ Validar')
```
- Ejecuta: `mvn validate`
- Valida estructura del proyecto

### 5. Compilar (🔧)
```groovy
stage('🔧 Compilar')
```
- Ejecuta: `mvn compile`
- Compila código fuente
- Genera clases en `target/classes/`

### 6. Ejecutar Tests (🧪)
```groovy
stage('🧪 Ejecutar Tests')
```
- Ejecuta: `mvn test`
- Corre pruebas unitarias
- Publica resultados con JUnit

### 7. Empaquetar (📦)
```groovy
stage('📦 Empaquetar')
```
- Ejecuta: `mvn package -DskipTests`
- Genera: `gh-1.0-SNAPSHOT.jar`
- Copia dependencias a `target/lib/`

### 8. Verificar Artifacts (🔍)
```groovy
stage('🔍 Verificar Artifacts')
```
- Lista el JAR generado
- Cuenta dependencias copiadas
- Muestra tamaño total

### 9. Análisis de Código (📊)
```groovy
stage('📊 Análisis de Código (Opcional)')
```
- Solo en branch master
- Checkstyle para análisis de estilo
- SonarQube (si está configurado)

### 10. Archivar Artifacts (📤)
```groovy
stage('📤 Archivar Artifacts')
```
- Archiva el JAR principal
- Archiva las dependencias
- Disponibles para descarga

### 11. Deploy (🚀)
```groovy
stage('🚀 Deploy a Dev (Opcional)')
```
- Solo en branch master
- Copia artifacts a servidor
- Reinicia servicio (ejemplo)

## 📦 Artifacts Generados

Después de un build exitoso, se generan:

```
Arquetipos/gh/target/
├── gh-1.0-SNAPSHOT.jar          # JAR ejecutable principal
├── lib/                          # Dependencias
│   ├── transport-rest-vertx-*.jar
│   ├── transport-highway-*.jar
│   ├── provider-springmvc-*.jar
│   ├── log4j-*.jar
│   └── ... (otras dependencias)
└── classes/                      # Clases compiladas
```

## 🚀 Ejecutar la Aplicación

### Desde Jenkins (Artifacts descargados)

```bash
# Descargar artifacts desde Jenkins
# Los encontrarás en: Build #X → Build Artifacts

# Ejecutar la aplicación
java -jar gh-1.0-SNAPSHOT.jar
```

### Desde el Workspace Local

```bash
# Navegar al proyecto
cd Arquetipos/gh

# Compilar y empaquetar
mvn clean package

# Ejecutar
java -jar target/gh-1.0-SNAPSHOT.jar
```

## ⚙️ Variables de Entorno

Puedes personalizar el pipeline con estas variables:

```groovy
environment {
    PROJECT_NAME = 'gh'
    MAVEN_OPTS = '-Xmx1024m'
    PROJECT_PATH = 'Arquetipos/gh'
}
```

## 🔧 Personalización del Pipeline

### Cambiar la Versión de Java

Si necesitas cambiar a Java 11 o superior:

```groovy
tools {
    maven 'Maven-3.8'  // o 'Maven-3.9'
    jdk 'Java-11'      // o 'Java-17', 'Java-21'
}
```

**Nota**: También deberás actualizar el `pom.xml`:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.1</version>
    <configuration>
        <source>11</source>  <!-- Cambiar -->
        <target>11</target>  <!-- Cambiar -->
    </configuration>
</plugin>
```

### Agregar Despliegue a Producción

```groovy
stage('🚀 Deploy a Producción') {
    when {
        branch 'master'
        // Requerir aprobación manual
        beforeInput true
    }
    input {
        message "¿Desplegar a producción?"
        ok "Desplegar"
    }
    steps {
        echo '🚀 Desplegando a producción...'
        // Comandos de despliegue
    }
}
```

### Agregar Notificaciones

```groovy
post {
    success {
        emailext (
            subject: "✅ Build Exitoso - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "El build se completó exitosamente.",
            to: "equipo@ejemplo.com"
        )
    }
    failure {
        emailext (
            subject: "❌ Build Fallido - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "El build falló. Revisa los logs.",
            to: "equipo@ejemplo.com"
        )
    }
}
```

## 🐛 Troubleshooting

### Error: "Unable to locate package temurin-8-jdk"

El contenedor ya incluye Java 8 (Temurin). Verifica que está instalado:

```bash
docker exec jenkins /usr/lib/jvm/temurin-8-jdk-amd64/bin/java -version
```

### Error: "Main class gh.Application not found"

Asegúrate de que:
1. El código fuente existe en `src/main/java/gh/Application.java`
2. El pom.xml tiene configurado correctamente el mainClass
3. El build fue exitoso

### Tests Fallan

Si los tests fallan, revisa:

```bash
# Ver resultados detallados
cat Arquetipos/gh/target/surefire-reports/*.txt
```

### Dependencias no se descargan

Verifica conexión a Maven Central:

```groovy
stage('Verificar Conexión') {
    steps {
        sh 'mvn dependency:resolve'
    }
}
```

## 📊 Métricas y Reportes

### Cobertura de Código (JaCoCo)

Agrega al `pom.xml`:

```xml
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.8</version>
    <executions>
        <execution>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

Y en el Jenkinsfile:

```groovy
post {
    always {
        jacoco(
            execPattern: '**/target/jacoco.exec',
            classPattern: '**/target/classes',
            sourcePattern: '**/src/main/java'
        )
    }
}
```

## 📚 Referencias

- [Apache ServiceComb](https://servicecomb.apache.org/)
- [Maven Documentation](https://maven.apache.org/guides/)
- [Jenkins Pipeline Syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)
- [Repositorio del Proyecto](https://github.com/cristianjonhson/CursoMaven)

## 🎯 Siguiente Pasos

1. ✅ Crear el Jenkinsfile en el repositorio
2. ✅ Configurar el job en Jenkins
3. ✅ Ejecutar el primer build
4. ✅ Verificar artifacts generados
5. ✅ Configurar notificaciones (opcional)
6. ✅ Agregar SonarQube (opcional)
7. ✅ Configurar despliegue automático (opcional)

---

**Autor**: Cristian Johnson  
**Proyecto**: CursoMaven - Arquetipo GH  
**Fecha**: 27 de diciembre de 2025
