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
                    
                    // Iterate over the credentials and use the certificate binding
                    creds.CERTS.each { cert ->
                        withCredentials([file(credentialsId: cert.ID, variable: 'CERT_FILE')]) {
                            // Output the .pem file to a temporary location
                            sh """
                            echo "PEM File Path: ${CERT_FILE}"
                            cp ${CERT_FILE} ${cert.FILE}
                            """
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
                    cat cert1.pem
                    cat cert2.crt
                    """
                }
            }
        }
    }
}
