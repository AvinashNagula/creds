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
    //     stage('Load Credentials') {
    //         steps {
    //             script {
    //                 // Load the credentials from the groovy file
    //                 def creds = load 'credentials.groovy'
                    
    //                 // Iterate over the credentials and use the certificate binding
    //                 creds.CERTS.each { cert ->
    //                     withCredentials([certificate(credentialsId: cert.ID, keystoreVariable: 'CERT_KEYSTORE', passwordVariable: 'CERT_PASSWORD')]) {
    //                         // Output the keystore path to a temporary location
    //                         sh "cp ${CERT_KEYSTORE} ${cert.FILE}"
    //                     }
    //                 }
    //             }
    //         }
    //    }
         stage('Load Credentials') {
            steps {
                script {
                    // Load the credentials from the groovy file
                    def creds = load 'credentials.groovy'
                    
                    // Iterate over the credentials and bind them
                    creds.CERTS.each { cert ->
                        withCredentials([certificate(credentialsId: cert.ID, keystoreVariable: 'CERT_KEYSTORE', passwordVariable: 'CERT_PASSWORD')]) {
                            // Make sure the keystore is initialized before usage
                            sh """
                            echo "Keystore Path: ${CERT_KEYSTORE}"
                            echo "Using Keystore Password: ${CERT_PASSWORD}"
                            # Assuming the keystore needs to be initialized or used for some command
                            # Example: You might use it to run a Java application
                            # java -Djavax.net.ssl.keyStore=${CERT_KEYSTORE} -Djavax.net.ssl.keyStorePassword=${CERT_PASSWORD} -jar your_app.jar
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
                    cat cert1.crt
                    cat cert2.crt
                    """
                }
            }
        }
    }
}
