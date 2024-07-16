pipeline {
    agent any
    
    tools{
        maven 'maven'
    }

    stages {
        stage('Build-code') {
            steps {
                sh 'mvn clean package'
            }
            post{
                success{
                    echo 'Archiving the Artifacts'
                    archiveArtifacts artifacts: '**/*.war'
                }
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
