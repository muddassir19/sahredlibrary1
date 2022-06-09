//Bild war file using maven and upload the war file to nexus repository.

pipeline {
    agent any
    tools{
        maven 'maven3'
    }
    stages {
        stage('Git Checkout'){
            steps{
                git branch: 'feature1-nexus', credentialsId: 'git-hub', 
                url: 'https://github.com/muddassir19/sahredlibrary1'
            }
        }
        stage('Build') {
            steps {
                sh script: 'mvn clean package'
            }
        }
        // Upload maven war file to the nexus repository
        stage('Upload war file nexus'){
            steps{
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'myweb', 
                        classifier: '', 
                        file: 'target/myweb-1.0.0.war', 
                        type: 'war'
                    ]
                ], 
                credentialsId: 'nexus3', 
                groupId: 'in.javahome', 
                nexusUrl: '172.31.0.46:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'javaapp-release/', 
                version: '1.0.0'
            }
        }
    }
}
