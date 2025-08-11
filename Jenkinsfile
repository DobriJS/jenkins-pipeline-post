pipeline {
    agent any

    stages {
        stage('Create directory for the WEB Application') {
            steps {
                echo "Cleaning and creating tomcat-web directory..."
                sh """
                    rm -rf "${WORKSPACE}/tomcat-web"
                    mkdir -p "${WORKSPACE}/tomcat-web"
                """
            }
        }

        stage('Drop the Apache Tomcat Docker container') {
            steps {
                echo 'Dropping the container if it exists...'
                sh 'docker rm -f tomcat1 || true'
                sh 'sleep 2' // brief pause to ensure cleanup
            }
        }

        stage('Create the Tomcat container') {
            steps {
                echo 'Creating the Tomcat container...'
                sh """
                    docker run -dit \
                        --name tomcat1 \
                        -p 9090:8080 \
                        -v "${WORKSPACE}/tomcat-web:/usr/local/tomcat/webapps" \
                        tomcat:9.0
                """
            }
        }

        stage('Copy the web application to the container directory') {
            steps {
                echo 'Preparing shopping app folder...'
                sh """
                    mkdir -p "${WORKSPACE}/tomcat-web/shopping"
                    cp -r shopping/* "${WORKSPACE}/tomcat-web/shopping/"
                """
            }
        }
    }

    post {
        always {
            echo 'These steps are always executed'
        }

        success {
            echo 'The deployment has worked'
            archiveArtifacts allowEmptyArchive: true, artifacts: 'shopping/**/*.jsp', followSymlinks: false
            cleanWs()
        }

        failure {
            echo 'An error has occurred'
        }
    }
}

