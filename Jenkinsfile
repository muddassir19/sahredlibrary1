@Library('sharedlibrary') _
pipeline {
    agent any

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
    }
     post {
    success {
      archiveArtifacts artifacts: 'target/*.war'
      cleanWs()
        }
     }
}
