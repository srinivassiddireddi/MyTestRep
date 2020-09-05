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
                sh 'mvn sonar:sonar -Dsonar.projectKey=java-app -Dsonar.host.url=http://a8412d93f8231484e846162e8cb7d17c-1964955021.us-east-1.elb.amazonaws.com -Dsonar.login=ea55bbe28c291adde5e39d2d2a511a516baadf24'
            }
        }
        stage ('upload artifactory') {
             steps {
                 script { 
                 def server = Artifactory.server 'jfrog'
                 def uploadSpec = """{
                    "files": [{
                       "pattern": "target/*.jar",
                       "target": "artifactory/libs-snapshot/"
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