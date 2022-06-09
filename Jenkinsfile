//@Library('sharedlibrary') _
pipeline {
    agent any
     environment {
       //SONAR_URL = "http://43.204.212.17:9000"
       DEV_SERVER = "172.31.5.237"
      } 

    stages {
        stage('maven build'){
            steps{
             sh 'mvn clean package'
            }
        }
        /*stage('Deploy to tomcat'){
            steps{
            tomcatDeploy('172.31.5.237', 'app', 'tomcat-dev')
            }
        } */
    /*    stage('sonarqube analysis'){
            steps{
                withSonarQubeEnv('sonar7') {
                 sh  'mvn sonar:sonar'
                 }
                waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                
           //  withCredentials([string(credentialsId: 'sonar-token', variable: 'sonartoken')]) {
               //  sh  "mvn sonar:sonar -Dsonar.host.url=${SONAR_URL} -Dsonar.login=${sonartoken}"      
                //  }
            }
        } */
        stage('Docker build'){
            steps{
               sh 'docker build . -t muddassir19/newapp:0.0.1'
            }
        }
        
         stage('Docker push image'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPsd')]) {
                    sh "docker login -u muddassir19 -p ${dockerHubPsd} "
                    }
               sh 'docker push muddassir19/newapp:0.0.1'
            }
        }
        stage('Running conatianer on dev server'){
            steps{
                def dockerRun = 'docker run -p 8181:8080 -d --name new-app muddassir19/newapp:0.0.1'
                sshagent(['tomcat-dev1']) {
                 sh "ssh -o StrictHostKeyChecking=no ec2-user@${DEV_SERVER} ${dockerRun}"
                }
                
            }
        }
    }
    /* post {
    success {
      archiveArtifacts artifacts: 'target/*.war'
      //cleanWs()
        }
     } */
}
