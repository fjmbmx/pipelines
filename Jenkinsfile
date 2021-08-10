pipeline{
    agent any
    tools {
        maven 'DefaultMaven'
        jdk 'DefaultJDK'
    }
    stages{
        stage('Clone sources') {
            steps {
                    git branch: 'main', url: 'https://github.com/fjmbmx/webflux-redis.git'
            }
        }
        stage("SonarQube") {

            steps{
                echo "starting codeAnalyze with SonarQube......"
                 // Tenga en cuenta que los parámetros en withSonarQubeEnv () deben ser los mismos que la configuración de Nombre en los servidores SonarQube antes
                withSonarQubeEnv('SonarQube') {
                    // Tenga en cuenta que los parámetros en withSonarQubeEnv () deben ser los mismos que la configuración de Nombre en los servidores SonarQube antes
                    withMaven() {
                        sh "mvn clean package -Dmaven.test.skip = true sonar:sonar -Dsonar.projectKey = santander_service -Dsonar.sourceEncoding =UTF-8 -Dsonar.exclusions = src/test/** -Dsonar.sources = src/ -Dsonar.java.binaries = target/classes -Dsonar.host.url =http://localhost:9000 -Dsonar.login =8ece15e8dcd076828dff39991a29e76c53672c9d"
                    }    
                }
                script {
                    timeout(1) {
                                 // Establezca el tiempo de espera aquí en 1 minuto, no aparecerá atascado en el estado de verificación
                                 // Usando la función de sonar webhook para notificar el resultado de la detección del código de la tubería, si no se supera el umbral de calidad, la tubería fallará
                        def qg = waitForQualityGate('SonarQube')
                                 // Nota: Los parámetros en waitForQualityGate () aquí también deberían ser los mismos que la configuración de Name en servidores SonarQube antes
                        if (qg.status != 'OK') {
                            error "No se pudo pasar la verificación del umbral de calidad del código de Sonarqube, ¡modifíquelo a tiempo! error"
                        }
                    }
                 }
            }
        }                 
        stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
    }
 }

 