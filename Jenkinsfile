pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    stages {
        stage('Build-code') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage ('Deploy to tomcat server') {
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat-deployer', path: '', url: 'http://54.165.72.144:8080/')], contextPath: null, war: '**/*.war'
            }
        }
        
    }
}
