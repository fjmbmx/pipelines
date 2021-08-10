pipeline{
    agent any
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
                    withMaven(maven: 'M3') {
                        sh "mvn clean package -Dmaven.test.skip = true sonar:sonar -Dsonar.projectKey = santander_service -Dsonar.sourceEncoding =UTF-8 -Dsonar.exclusions = src/test/** -Dsonar.sources = src/ -Dsonar.java.binaries = target/classes -Dsonar.host.url =http://localhost:9000 -Dsonar.login =e3b8996f8620f2402b38b30251d7ab393c275d5a"
                }    
        
                script {
                    timeout(1) {
                                 // Establezca el tiempo de espera aquí en 1 minuto, no aparecerá atascado en el estado de verificación
                                 // Usando la función de sonar webhook para notificar el resultado de la detección del código de la tubería, si no se supera el umbral de calidad, la tubería fallará
                        def qg = waitForQualityGate('SonarQube')
                                 // Nota: Los parámetros en waitForQualityGate () aquí también deberían ser los mismos que la configuración de Name en servidores SonarQube antes
                        if (qg.status != 'OK') {
                            error "No se pudo pasar la verificación del umbral de calidad del código de Sonarqube, ¡modifíquelo a tiempo! error: $ {qg.status}"
                        }
                    }
                 }
                }
        }


                  //  sh 'mvn sonar:sonar -Dsonar.host.url=http://0.0.0.0:9000/ -Dsonar.credentials=login:fdb5f436dc22040e0604932f5702b455a7770e80'
                 
        stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
    }
 }

 