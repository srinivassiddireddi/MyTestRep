pipeline{
    agent {label 'Slave'}
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
        stage ('Test Java COde_New_99') {
            steps {
                sh 'mvn test'
            }
        }
    }
}
