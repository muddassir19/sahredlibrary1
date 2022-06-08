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
        stage('Docker build'){
            steps{
               sh 'docker build . -t muddassir19/myweb-0.0.4:0.0.1'
            }
        }
        stage('sonarqube analysis'){
            steps{
                withSonarQubeEnv('sonar7') {
                 sh  'mvn sonar:sonar'
                 }
                waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                
           //  withCredentials([string(credentialsId: 'sonar-token', variable: 'sonartoken')]) {
               //  sh  "mvn sonar:sonar -Dsonar.host.url=${SONAR_URL} -Dsonar.login=${sonartoken}"      
                //  }
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
