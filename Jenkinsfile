pipeline {
     agent {label "slave1"}
    stages {
        stage('BUILD') {
            steps {
                script{
                    if (env.BRANCH_NAME == 'release') {
                        withCredentials([usernamePassword(credentialsId: 'docker_hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) { 

                            sh """

                                docker build .  -t omarkorety/bakehouse:${BUILD_NUMBER}
                                echo ${BUILD_NUMBER}
                                docker login -u ${USERNAME} -p ${PASSWORD}
                                docker push omarkorety/bakehouse:${BUILD_NUMBER}
                                echo ${BUILD_NUMBER} > ../build_num.txt
                                """
                    }
                        } else {
                            echo 'I execute elsewhere'
                        }
                }

                }
            }
        
        stage('DEPLOY') {
            steps {
                script{
                     if (env.BRANCH_NAME == 'test' || env.BRANCH_NAME == 'dev'|| env.BRANCH_NAME == 'prod') {


                        withCredentials([file(credentialsId: 'kubernates_config', variable: 'kubecfg')]){
                            // sh "kubectl config set-context $(kubectl config current-context)"   // --namespace=${namespace}

                            sh """
                                export BUILD_NUMBER=\$(cat ../build_num.txt)
                                mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                                cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                                rm -f Deployment/deploy.yaml.tmp
                                kubectl apply --kubeconfig=${kubecfg} -f Deployment

                                """
                }
                        } else {
                            echo 'I execute elsewhere'
                        }

                }                                                   
                }     
            }
    }         
    }
    

   
