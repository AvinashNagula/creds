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
                        withCredentials([certificate(credentialsId: cert.ID, keystoreVariable: 'CERT_KEYSTORE', passwordVariable: 'CERT_PASSWORD')]) {
                            // Output the keystore path to a temporary location
                            sh "cp ${CERT_KEYSTORE} ${cert.FILE}"
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
                    """
                }
            }
        }
    }
}
