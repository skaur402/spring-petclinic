pipeline {

    triggers {
        cron('H/5 * * * 4')
    }

    tools {
        jdk 'JDK11'       
        maven 'Maven3'    
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/skaur402/spring-petclinic.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test & Jacoco') {
            steps {
                sh 'mvn test jacoco:report'
            }
            post {
                always {
                    // Generate Jacoco coverage report
                    jacoco execPattern: '**/target/jacoco.exec'
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'target/site/jacoco',
                        reportFiles: 'index.html',
                        reportName: 'Jacoco Code Coverage'
                    ])
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}
