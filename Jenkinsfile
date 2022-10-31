pipeline {
    agent any
    stages {
        stage('Example Username/Password') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) { 
                
                sh """
                    
                    docker build .  -t omarkorety/bakehouse:$BUILD_NUMBER
                    docker login -u ${USERNAME} -p ${PASSWORD}
                    docker push omarkorety/bakehouse:$BUILD_NUMBER
                    """
                }
            }
        
    
                    
            }
        }
//         stage('Example SSH Username with private key') {
//             environment {
//                 SSH_CREDS = credentials('my-predefined-ssh-creds')
//             }
//             steps {
//                 sh 'echo "SSH private key is located at $SSH_CREDS"'
//                 sh 'echo "SSH user is $SSH_CREDS_USR"'
//                 sh 'echo "SSH passphrase is $SSH_CREDS_PSW"'
//             }
//         }
    }

