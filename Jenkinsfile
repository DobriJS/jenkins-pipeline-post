pipeline {
    agent any

    stages {
        stage('Create directory for the WEB Application') {
            steps {
                // First, drop the directory if it exists
                sh 'rm -rf $(pwd)/tomcat-web'
                // Create the directory
                sh 'mkdir $(pwd)/tomcat-web'
            }
        }

        stage('Drop the Apache Tomcat Docker container') {
            steps {
                echo 'Dropping the container...'
                sh 'docker rm -f tomcat1 || true' // avoid failure if not running
            }
        }

        stage('Create the Tomcat container') {
            steps {
                echo 'Creating the container...'
                sh 'docker run -dit --name tomcat1 -p 9090:8080 -v $(pwd)/tomcat-web:/usr/local/tomcat/webapps tomcat:9.0'
            }
        }

        stage('Copy the web application to the container directory') {
            steps {
                echo 'Creating the shopping folder in the container'
                sh 'mkdir -p $(pwd)/tomcat-web/shopping'

                echo 'Copying web application...'
                sh 'cp -r shopping/* $(pwd)/tomcat-web/shopping'
            }
        }
    }

    post {
        always {
            echo 'These steps are always executed'
        }

        success {
            echo 'The deployment has worked'
            archiveArtifacts allowEmptyArchive: true, artifacts: 'shopping/*.jsp', followSymlinks: false
            cleanWs()
        }

        failure {
            echo 'An error has occurred'
        }
    }
}

