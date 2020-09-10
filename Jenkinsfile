pipeline{
    agent any
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
    }
}