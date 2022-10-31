pipeline {
    agent any
    stages {
        stage('Example Username/Password') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) { 
                
                sh """
                    
                    docker build .  -t omarkorety/bakehouse:$BUILD_NUMBER
                    docker login -u ${USERNAME} -p ${PASSWORD}
                    docker push omarkorety/bakehouse:$BUILD_NUMBER
                    """
                }
            }
        
    
                    
            }
        }
        stage('CD') {
            steps {
                    
                 // sh "docker run -d -p 3000:3000 ahmedhedihed/bakehouse:$BUILD_NUMBER"
                 
                withCredentials([file(credentialsId: '	kubernates_config', variable: 'kubecfg')]){
                    // Change context with related namespace
                    // sh "kubectl config set-context $(kubectl config current-context)"   // --namespace=${namespace}

                      // sh 'cat $kubecfg > ~/.kube/config'
                       sh """
                            
                            kubectl apply --kubeconfig=${kubecfg} -f Deployment
 
                            
                       
                       """

                        
                    
                }
                 
                 
                 
            }     

            
        }         
   

   
   
        
        
        
        
        
    }

   

