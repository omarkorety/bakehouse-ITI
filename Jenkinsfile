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
                                  sed -i 's/omar/${env.BUILD_NUMBER}/g' Deployment/deploy.yaml
                                  kubectl apply -f Deployment -n jenkins




                                """
                }
                        }

                }                                                   
                }     
    }         
    }
    

   
