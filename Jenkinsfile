pipeline {
  agent any
  environment {
    SONAR_TOKEN = sqp_389a532309e6840bf3ce71d71a7b8bc56b12f39c
  }
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Install') {
      steps { sh 'gradlew -Dhttps.proxyHost=proxy1-rech -Dhttps.proxyPort=3128' }
    }
    stage('Build') {
      steps { sh 'npm run build' }
    }
    
    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          steps{sh './gradlew sonar \
  -Dsonar.projectKey=Jacoco \
  -Dsonar.projectName='Jacoco' \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.token=sqp_8867b36a25f37910d8e2684a323fa61eb6d97708'
                ]
        }
      }
    }
  }
