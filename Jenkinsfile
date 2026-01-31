node {

    stage('Checkout') {
        checkout scm
    }

    stage('Build & Test') {
        sh './gradlew clean build'
    }

    stage('SonarQube Analysis') {
        withSonarQubeEnv('SonarQube') {
            sh './gradlew sonarqube'
        }
    }

}
