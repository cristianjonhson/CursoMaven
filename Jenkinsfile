// Pipeline para proyecto CursoMaven - Arquetipo gh
// Proyecto Java 8 con Apache ServiceComb
pipeline {
    agent any
    
    tools {
        // Este proyecto usa Java 8 (configurado en pom.xml)
        maven 'Maven-3.6'
        jdk 'Java-8'
    }
    
    environment {
        // Variables del proyecto
        PROJECT_NAME = 'gh'
        MAVEN_OPTS = '-Xmx1024m'
        // Ruta del proyecto (ajustar según tu repo)
        PROJECT_PATH = 'Arquetipos/mavenholamundo'
    }
    
    stages {
        stage('📋 Información del Entorno') {
            steps {
                echo '=== Versiones de Herramientas ==='
                sh 'java -version'
                sh 'mvn -version'
                sh 'echo "Workspace: $WORKSPACE"'
            }
        }
        
        stage('📥 Checkout') {
            steps {
                echo '📥 Clonando repositorio...'
                git branch: 'master',
                    url: 'https://github.com/cristianjonhson/CursoMaven.git'
            }
        }
        
        stage('🧹 Clean') {
            steps {
                echo '🧹 Limpiando artifacts anteriores...'
                dir("${PROJECT_PATH}") {
                    sh 'mvn clean'
                }
            }
        }
        
        stage('✅ Validar') {
            steps {
                echo '✅ Validando estructura del proyecto...'
                dir("${PROJECT_PATH}") {
                    sh 'mvn validate'
                }
            }
        }
        
        stage('🔧 Compilar') {
            steps {
                echo '🔧 Compilando código fuente...'
                dir("${PROJECT_PATH}") {
                    sh 'mvn compile'
                }
            }
        }
        
        stage('🧪 Ejecutar Tests') {
            steps {
                echo '🧪 Ejecutando pruebas unitarias...'
                dir("${PROJECT_PATH}") {
                    sh 'mvn test'
                }
            }
            post {
                always {
                    // Publicar resultados de tests si existen
                    junit allowEmptyResults: true, 
                         testResults: "${PROJECT_PATH}/**/target/surefire-reports/*.xml"
                }
            }
        }
        
        stage('📦 Empaquetar') {
            steps {
                echo '📦 Empaquetando aplicación...'
                dir("${PROJECT_PATH}") {
                    // El pom.xml tiene configurado:
                    // - maven-jar-plugin: Crea el JAR ejecutable
                    // - maven-dependency-plugin: Copia dependencias a target/lib
                    sh 'mvn package -DskipTests'
                }
            }
        }
        
        stage('🔍 Verificar Artifacts') {
            steps {
                echo '🔍 Verificando artifacts generados...'
                dir("${PROJECT_PATH}") {
                    sh '''
                        echo "Contenido de target:"
                        ls -lh target/*.jar || echo "No se encontraron JARs"
                        
                        echo ""
                        echo "Dependencias copiadas:"
                        ls -lh target/lib/*.jar | wc -l || echo "No se encontraron dependencias"
                        
                        echo ""
                        echo "Tamaño total de la aplicación:"
                        du -sh target/
                    '''
                }
            }
        }
        
        stage('📊 Análisis de Código (Opcional)') {
            when {
                branch 'master'
            }
            steps {
                echo '📊 Ejecutando análisis de código...'
                dir("${PROJECT_PATH}") {
                    // Descomentar si tienes SonarQube configurado
                    // sh 'mvn sonar:sonar'
                    
                    // Análisis de estilo de código
                    sh 'mvn checkstyle:checkstyle || true'
                }
            }
        }
        
        stage('📤 Archivar Artifacts') {
            steps {
                echo '📤 Archivando artifacts...'
                // Archivar el JAR principal
                archiveArtifacts artifacts: "${PROJECT_PATH}/target/*.jar",
                                fingerprint: true,
                                allowEmptyArchive: false
                
                // Opcional: Archivar las dependencias
                archiveArtifacts artifacts: "${PROJECT_PATH}/target/lib/*.jar",
                                fingerprint: true,
                                allowEmptyArchive: true
            }
        } 
    }
    
    post {
        
        success {
            echo '✅ Pipeline completado exitosamente!'
            echo "Artifact generado: ${PROJECT_NAME}-1.0-SNAPSHOT.jar"
            echo "Main class: gh.Application"
            
            // Opcional: Notificación de éxito
            // emailext subject: "✅ Build Exitoso - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            //          body: "El build se completó exitosamente.",
            //          to: "equipo@ejemplo.com"
        }
        
        failure {
            echo '❌ Pipeline falló!'
            
            // Opcional: Notificación de fallo
            // emailext subject: "❌ Build Fallido - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            //          body: "El build falló. Por favor revisa los logs.",
            //          to: "equipo@ejemplo.com"
        }
        
        unstable {
            echo '⚠️ Build inestable (tests fallaron)'
        }
    }
}
