@Library('sharedlibrary') _
pipeline {
    agent any
    environment {
       SONAR_URL = "http://43.204.212.17:9000"
      }

    stages {
        stage('maven build'){
            steps{
             sh 'mvn clean package'
            }
        }
        stage('Deploy to tomcat'){
            steps{
            tomcatDeploy('172.31.5.237', 'app', 'tomcat-dev')
            }
        }
        stage('sonarqube analysis'){
            steps{
             withCredentials([string(credentialsId: 'sonar-token', variable: 'sonartoken')]) {
                 sh  "mvn sonar:sonar -Dsonar.host.url=${SONAR_URL} -Dsonar.login=${sonartoken}"      
                  }
            }
        }
    }
     post {
    success {
      archiveArtifacts artifacts: 'target/*.war'
      cleanWs()
        }
     }
}
