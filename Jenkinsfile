pipeline {
    agent any

    environment {
        // Add Maven to the PATH
        PATH = "/opt/maven/bin:$PATH"
    }

    stages {
        stage('Build-code') {
            steps {
                echo 'Building the project...'
                sh 'mvn clean package'
            }
        }
        stage('Push to Repository') {
            steps {
                sh 'mvn deploy'
            }
        }
        stage ('Deploy to tomcat server') {
            steps{
                echo 'Deploying to Tomcat server...'
                deploy adapters: [
                    tomcat9(
                        credentialsId: 'tomcat-deployer', 
                        path: '', 
                        url: 'http://54.165.72.144:8080/'
                    )
                ], 
                contextPath: null,
                war: '**/*.war'
            }
        }
        
    }
    post {
        always {
            echo 'Pipeline execution finished.'
        }
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
