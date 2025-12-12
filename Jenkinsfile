pipeline {
agent any
environment {
  SONAR_HOST_URL = 'http://localhost:9000'
  SONAR_TOKEN = 'sqp_8867b36a25f37910d8e2684a323fa61eb6d97708'
  }
stages {
  stage('Checkout') {
    steps {
    checkout scm
    }
  }

  
stage('Build') {
    steps {
      sh './gradlew build -Dhttps.proxyHost="proxy1-rech" -Dhttps.proxyPort=3128 -x test'
    }
  }
  stage('SonarQube Analysis') {
    steps {
      withCredentials([string(credentialsId: 'Jacoco', variable: 'SONAR_TOKEN')]) {
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
      junit '/src/test/java/gipf//.xml'
      archiveArtifacts artifacts: '**/build/libs/.jar', allowEmptyArchive: true
    }
  }  
}
