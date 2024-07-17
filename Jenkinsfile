def registry = 'https://keerthi03.jfrog.io/'
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
                sh 'mvn clean install'
            }
            post{
                success{
                    echo 'Archiving the Artifacts'
                    archiveArtifacts artifacts: '**/*.war'
                }
                failure {
                    echo 'Archiving the Artifacts failed.'
                }
            }

        }
        stage ('Jar Publish'){
            steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"jfrog-cred"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "**/*.war",
                              "target": "mvn-libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}"
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
                }
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
