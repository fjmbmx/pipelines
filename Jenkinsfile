pipeline{
    agent any

    stages{
            stage('Clone sources') {
                steps {
                    git branch: 'main', url: 'https://github.com/fjmbmx/webflux-redis.git'
                }
            }
            stage('SonarQube analysis') {
                steps {
                    withSonarQubeEnv('SonarQube') {
                        sh "./gradlew sonarqube"
                    }
                }
            }
            stage("Quality gate") {
                steps {
                    waitForQualityGate abortPipeline: true
                }
            }
            stage('Clean') {}
            stage('Build') {}
            stage('Test') {}

            stage('Deploy - Test') {}
            stage('Deploy - Production') {}
            stage('Deliver') {}
        }
 }

 