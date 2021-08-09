pipeline{
    agent any

    stages{
        stage('Clone sources') {
            steps {
                    git branch: 'main', url: 'https://github.com/fjmbmx/webflux-redis.git'
                }
            }
        stage("maven sonar") {
            steps{
                withMaven() {
                    sh 'mvn sonar:sonar -Dsonar.host.url=http://http://0.0.0.0:9000/ -Dsonar.credentials=login:6ca02289d0fb3cd5433c031420889da3bd090362'
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

 