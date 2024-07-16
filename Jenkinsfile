pipeline {
    agent any
    
    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    stages {
        stage('clone-code') {
            steps {
                git branch: 'master', url: 'https://github.com/Keerthi-Korrapati/hello-world.git'
            }
        }
        stage('Build-code') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Deploy') {
            steps {
                sshagent(['deploy-user1']) {
                    sh "ssh-keyscan -H 54.242.118.220 >> ~/.ssh/known_hosts"
                    sh "scp webapp/target/wepapp.war -o StrictHostKeyChecking=no ec2-user@54.242.118.220:/opt/tomcat/webapps"
    
                }
            }
        }
        
    }
}
