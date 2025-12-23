# 📚 CursoMaven

Repositorio de aprendizaje de Apache Maven con ejemplos prácticos de arquetipos y proyectos básicos Java.

## 📋 Tabla de Contenidos

- [Descripción](#descripción)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Tecnologías y Versiones](#tecnologías-y-versiones)
- [Requisitos Previos](#requisitos-previos)
- [Instalación](#instalación)
- [Uso](#uso)
- [Proyectos Incluidos](#proyectos-incluidos)
- [Comandos Útiles de Maven](#comandos-útiles-de-maven)
- [Licencia](#licencia)

## 📖 Descripción

Este repositorio contiene proyectos de ejemplo creados con diferentes arquetipos de Maven para fines educativos. Incluye desde un simple "Hello World" hasta un microservicio básico usando Apache ServiceComb Java Chassis.

## 🗂️ Estructura del Proyecto

```
CursoMaven/
├── .gitignore                          # Archivos y directorios ignorados por Git
├── README.md                           # Este archivo
└── Arquetipos/                         # Directorio principal de arquetipos
    ├── mavenholamundo/                 # Proyecto básico Maven quickstart
    │   ├── pom.xml                     # Archivo de configuración Maven
    │   └── src/
    │       ├── main/
    │       │   └── java/
    │       │       └── quickstart.maven/
    │       │           └── App.java    # Aplicación "Hello World"
    │       └── test/
    │           └── java/
    │               └── quickstart-maven/
    │                   └── AppTest.java # Pruebas unitarias
    │
    └── gh/                             # Microservicio ServiceComb
        ├── pom.xml                     # Configuración Maven con dependencias ServiceComb
        ├── README.md                   # Documentación específica del microservicio
        └── src/
            └── main/
                ├── java/
                │   └── gh/
                │       ├── Application.java      # Clase principal del microservicio
                │       ├── HelloImpl.java        # Implementación REST endpoint
                │       └── HelloConsumer.java    # Consumidor de servicio
                └── resources/
                    ├── log4j2.xml               # Configuración de logging
                    └── microservice.yaml        # Configuración del microservicio
```

## 🛠️ Tecnologías y Versiones

### Proyecto: mavenholamundo

| Tecnología | Versión |
|-----------|---------|
| Java | 1.7+ |
| Maven | 3.x |
| JUnit | 4.11 |
| Maven Compiler Plugin | 3.8.0 |
| Maven Surefire Plugin | 2.22.1 |

### Proyecto: gh (ServiceComb)

| Tecnología | Versión |
|-----------|---------|
| Java | 8+ |
| Maven | 3.x |
| Apache ServiceComb Java Chassis | 2.1.2.RELEASE |
| Log4j2 | (gestionado por ServiceComb) |
| Spring MVC Provider | (incluido en Java Chassis) |

## 📦 Requisitos Previos

Antes de ejecutar los proyectos, asegúrate de tener instalado:

- **Java Development Kit (JDK)**: versión 1.7 o superior (se recomienda Java 8+ para el proyecto ServiceComb)
- **Apache Maven**: versión 3.x o superior
- **Git**: para clonar el repositorio

### Verificar instalaciones

```bash
# Verificar Java
java -version

# Verificar Maven
mvn -version

# Verificar Git
git --version
```

## 💿 Instalación

### Clonar el repositorio

```bash
git clone https://github.com/cristianjonhson/CursoMaven.git
cd CursoMaven
```

## 🚀 Uso

### Proyecto 1: mavenholamundo (Hello World básico)

#### Compilar el proyecto

```bash
cd Arquetipos/mavenholamundo
mvn clean compile
```

#### Ejecutar pruebas

```bash
mvn test
```

#### Empaquetar (crear JAR)

```bash
mvn package
```

#### Ejecutar la aplicación

```bash
# Opción 1: Ejecutar directamente
mvn exec:java -Dexec.mainClass="quickstart.maven.App"

# Opción 2: Ejecutar el JAR compilado
java -cp target/mavenholamundo-1.0-SNAPSHOT.jar quickstart.maven.App
```

**Salida esperada:**
```
Hello World!
```

### Proyecto 2: gh (Microservicio ServiceComb)

#### Compilar el proyecto

```bash
cd Arquetipos/gh
mvn clean compile
```

#### Empaquetar el microservicio

```bash
mvn package
```

Este comando generará:
- `target/gh-1.0-SNAPSHOT.jar` - Archivo JAR ejecutable
- `target/lib/` - Directorio con todas las dependencias

#### Ejecutar el microservicio

```bash
java -jar target/gh-1.0-SNAPSHOT.jar
```

#### Probar el endpoint REST

Una vez que el microservicio esté en ejecución, puedes probar el endpoint:

```bash
# Usando curl
curl http://localhost:8080/hello

# O usando un navegador
open http://localhost:8080/hello
```

**Salida esperada:**
```
Hello World!
```

#### Configuración del microservicio

Puedes modificar la configuración en `src/main/resources/microservice.yaml` para cambiar:
- ID de la aplicación
- Nombre del servicio
- Puerto del servidor
- Dirección del Service Center
- Endpoints de publicación

## 📚 Proyectos Incluidos

### 1. mavenholamundo

**Arquetipo utilizado:** `maven-archetype-quickstart`

**Descripción:** Proyecto básico de Java que demuestra la estructura mínima de un proyecto Maven. Incluye:
- Una clase principal con un método `main`
- Configuración básica de Maven
- Dependencia de JUnit para pruebas unitarias
- Plugins estándar de Maven

**Propósito educativo:** Entender la estructura básica de un proyecto Maven y el ciclo de vida de construcción.

### 2. gh (ServiceComb Microservice)

**Arquetipo utilizado:** `org.apache.servicecomb.archetypes:business-service-springmvc-archetype`

**Descripción:** Microservicio completo basado en Apache ServiceComb Java Chassis. Incluye:
- REST API con Spring MVC
- Configuración de logging con Log4j2
- Gestión de transporte REST y Highway
- Arquitectura de microservicios lista para producción

**Propósito educativo:** Introducción a arquitecturas de microservicios con ServiceComb.

## 📝 Comandos Útiles de Maven

### Comandos básicos

```bash
# Limpiar el proyecto (eliminar target/)
mvn clean

# Compilar el código fuente
mvn compile

# Ejecutar pruebas unitarias
mvn test

# Empaquetar en JAR/WAR
mvn package

# Instalar en repositorio local
mvn install

# Compilar sin ejecutar pruebas
mvn clean install -DskipTests

# Ver árbol de dependencias
mvn dependency:tree

# Actualizar dependencias
mvn versions:display-dependency-updates

# Verificar el proyecto
mvn verify
```

### Ciclo de vida de Maven

Maven define tres ciclos de vida estándar:

1. **clean**: Limpieza del proyecto
   - `pre-clean` → `clean` → `post-clean`

2. **default**: Construcción del proyecto
   - `validate` → `compile` → `test` → `package` → `verify` → `install` → `deploy`

3. **site**: Generación de documentación
   - `pre-site` → `site` → `post-site` → `site-deploy`

### Comandos avanzados

```bash
# Generar proyecto desde arquetipo
mvn archetype:generate -DgroupId=com.ejemplo -DartifactId=mi-proyecto -DarchetypeArtifactId=maven-archetype-quickstart

# Ejecutar una clase específica
mvn exec:java -Dexec.mainClass="paquete.ClasePrincipal"

# Análisis de código con CheckStyle
mvn checkstyle:check

# Generar reporte de cobertura de código
mvn jacoco:report

# Limpiar, compilar y empaquetar en un solo comando
mvn clean package
```

## 📄 Licencia

Los proyectos incluidos en este repositorio utilizan diferentes licencias:

- **Proyecto gh**: Apache License 2.0 (heredado de Apache ServiceComb)
- **Proyecto mavenholamundo**: Proyecto educativo sin licencia específica

## 🤝 Contribuciones

Este es un proyecto educativo. Si encuentras errores o tienes sugerencias de mejora, no dudes en:

1. Abrir un **Issue**
2. Enviar un **Pull Request**
3. Compartir tus comentarios

## 📚 Recursos Adicionales

### Documentación oficial

- [Apache Maven](https://maven.apache.org/)
- [Maven Getting Started Guide](https://maven.apache.org/guides/getting-started/)
- [Apache ServiceComb](http://servicecomb.apache.org/)
- [Java Tutorials](https://docs.oracle.com/javase/tutorial/)

### Tutoriales recomendados

- [Maven in 5 Minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)
- [POM Reference](https://maven.apache.org/pom.html)
- [Dependency Mechanism](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html)

---

⭐ **¿Te resultó útil este repositorio?** ¡Dale una estrella en GitHub!

💬 **¿Tienes preguntas?** Abre un issue y con gusto te ayudaremos.

---

**Desarrollado por:** [cristianjonhson](https://github.com/cristianjonhson)

**Fecha:** Diciembre 2025
