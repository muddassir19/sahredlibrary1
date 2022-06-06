@Library('sharedlibrary') _
pipeline {
    agent any

    stages {
        stage('git checkout') {
            steps {
                git credentialsId: 'git-hub', url: 'https://github.com/muddassir19/sahredlibrary1'
             //git branch: 'tomcat-ci-cd', credentialsId: 'git-hub', url: 'https://github.com/muddassir19/my-app'
            }
        }
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
