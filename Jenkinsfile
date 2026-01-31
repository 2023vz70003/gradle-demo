pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh './gradlew clean build'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    withCredentials([
                        string(
                            credentialsId: 'sqa_0a380e9edd3d74f6ee9ad5f526baf0cd37ddaeb1',
                            variable: 'SONAR_TOKEN'
                        )
                    ]) {
                        sh '''
                          sonar-scanner \
                            -Dsonar.projectKey=gradle-demo \
                            -Dsonar.projectName=gradle-demo \
                            -Dsonar.sources=src/main/java \
                            -Dsonar.tests=src/test/java \
                            -Dsonar.java.binaries=build/classes \
                            -Dsonar.coverage.jacoco.xmlReportPaths=build/reports/jacoco/test/jacocoTestReport.xml \
                            -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }
    }
}
