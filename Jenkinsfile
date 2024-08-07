pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }
        stage('Checkout') {
            
            steps {
                // Checkout code from a Git repository
                git url: 'https://github.com/AvinashNagula/creds.git', branch: 'main'

            }
        }
        stage('Load Credentials') {
            steps {
                script {
                    // Load the credentials from the groovy file
                    def creds = load 'credentials.groovy'
                    
                    // Iterate over the credentials and write them to temporary files
                    creds.CERTS.each { cert ->
                        withCredentials([file(credentialsId: cert.ID, variable: 'CERT_CONTENT')]) {
                            writeFile file: cert.FILE, text: "${CERT_CONTENT}"
                        }
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // List the files to confirm they are written correctly
                    sh """
                    ls -l
                    cat cert1.crt
                    cat cert2.crt
                    cat secret
                    """
                }
            }
        }
    }
}
