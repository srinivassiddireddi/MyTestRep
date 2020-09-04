pipeline{
    agent {label 'slave'}
    tools{
        jdk 'jdk'
        maven 'mvn'
    }
    
    stages {
        stage ('complie java code') {
            steps {
                sh 'mvn clean'
                sh 'mvn install'
            }
        }
        stage ('Test Java COde') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('Sonarcube evalution'){
            steps {
                sh 'mvn sonar:sonar -Dsonar.projectKey=java-app -Dsonar.host.url=http://a8412d93f8231484e846162e8cb7d17c-1964955021.us-east-1.elb.amazonaws.com -Dsonar.login=e45ce196bed8022babc604e3eff00956b76b120e'
            }
        }
        stage ('upload artifactory') {
             steps {
                 script { 
                 def server = Artifactory.server 'art-1'
                 def uploadSpec = """{
                    "files": [{
                       "pattern": "target/*.jar",
                       "target": "libs-snapshot/"
                    }]
                 }"""
                 server.upload(uploadSpec) 
               }
             }
        }
        stage ('docker build') {
            steps {
                sh 'docker build -t java-pro .'
            }
        }
        
    }
}