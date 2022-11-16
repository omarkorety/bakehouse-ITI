pipeline {
     agent any
    stages {
        stage('BUILD') {
            steps {
                script{
                        withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) { 

                            sh """

                                docker build .  -t omarkorety/bakehouse:${BUILD_NUMBER}
                                echo ${BUILD_NUMBER}
                                docker login -u ${USERNAME} -p ${PASSWORD}
                                docker push omarkorety/bakehouse:${BUILD_NUMBER}
                                echo ${BUILD_NUMBER} > ../build_num.txt
                                """
                    }
                        } 
                }

            }
        
        stage('DEPLOY') {
            steps {
                script{


                        withCredentials([file(credentialsId: 'test-bazzzzzz', variable: 'test')]){
                            // sh "kubectl config set-context $(kubectl config current-context)"   // --namespace=${namespace}

                            sh """
                                 gcloud auth activate-service-account --key-file="$test"
                                 gcloud container clusters get-credentials my-gke-cluster --zone asia-east1-a --project omars-project-367822

                                  sed -i 's/omar/${env.BUILD_NUMBER}/g' Deployment/deploy.yaml




                                """
                }
                        }

                }                                                   
                }     
    }         
    }
    

   
