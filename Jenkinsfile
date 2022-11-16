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
                                 gcloud auth activate-service-account --key-file="$serviceAcc"
                                 gcloud container clusters get-credentials my-gke-cluster --zone asia-east1-a --project omars-project-367822
                                 export BUILD_NUMBER=\$(cat ../build_num.txt)
                                mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                                cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                                rm -f Deployment/deploy.yaml.tmp
                               kubectl apply -f Deployment -n jenkins



                                """
                }
                        }

                }                                                   
                }     
    }         
    }
    

   
