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
                    sh 'mvn sonar:sonar -Dsonar.host.url=http://0.0.0.0:9000/ -Dsonar.credentials=login:fdb5f436dc22040e0604932f5702b455a7770e80'
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

 