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
                    
                    // Iterate over the credentials and set environment variables dynamically
                    creds.CERTS.each { cert ->
                        env[cert.NAME] = credentials(cert.ID)
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Write the credentials to temporary files
                    def creds = load 'credentials.groovy'  // Reload if needed
                    creds.CERTS.each { cert ->
                        writeFile file: cert.FILE, text: "${env[cert.NAME]}"
                    }

                    // Build Docker image
                    sh """
                    ls -l
                    cat cert1.crt
                    cat cert2.crt
                    """
                }
            }
        }
    }
}
