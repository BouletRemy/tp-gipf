pipeline {
agent any
environment {
  SONAR_HOST_URL = 'http://localhost:9000'
  }
stages {
  stage('Checkout') {
    steps {
    checkout scm
    }
  }

  
stage('Build') {
    steps {
      sh './gradlew build -Dhttps.proxyHost="proxy1-rech" -Dhttps.proxyPort=3128'
    }
  }
  stage('SonarQube Analysis') {
    steps {
      withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
      withSonarQubeEnv('SonarQube') {
      sh '''
./gradlew sonarqube
-Dsonar.projectKey=Jacoco
-Dsonar.projectName=Jacoco
-Dsonar.host.url=${SONAR_HOST_URL}
-Dsonar.login=${SONAR_TOKEN}
'''
        }
      }
    }
  }
}
  post {
    always {
      junit '/build/test-results//.xml'
      archiveArtifacts artifacts: '**/build/libs/.jar', allowEmptyArchive: true
    }
  }  
}
