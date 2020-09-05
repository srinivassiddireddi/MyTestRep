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
        stage ('Upload artifactory') {
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
        stage ('Docker build') {
            steps {
                sh 'docker build -t java-pro .'
            }
        }
        stage ('Uplod to ECR') {
            steps{
                sh '''
                $(aws ecr get-login --region us-east-1 --no-include-email)
                docker tag java-app:latest 558607277863.dkr.ecr.us-east-1.amazonaws.com/java-app:latest
                docker push 558607277863.dkr.ecr.us-east-1.amazonaws.com/java-app:latest
                '''
            }
        }
        stage ('Deploy to kubernetes'){
            steps{
                git credentialsId: 'new-git', url: 'http://gitlab.stonehome.in/root/kuberenetes-files.git'
                sh '''
                aws eks update-kubeconfig --name myeks
                kubectl apply -f java-app/java-dp.yaml
                kubectl apply -f java-app/java-service.yaml
                '''
            }
        }
        
    }
}