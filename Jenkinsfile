pipeline{
    agent any 
    environment{
        REPOSITORY = "cherry98/first_app"
        
    }

    stages{
        stage("Scm Checkout"){
           steps{
                git credentialsId: 'ff790018-b09e-4fe7-ba7b-e02acfecc631', url: 'https://github.com/Bcharan-s/Docker_jenkins.git', branch: 'main'
            
        
            }
        }


                stage("Docker Build Image"){
                    steps{
                        script{
                            sh "docker build -t ${REPOSITORY}:V0..${BUILD_NUMBER} ."
                            echo "IMAGE BUILD SUCCESSFUL"

                        }
        
                    }
                }
                stage("Docker Push Image"){
                    steps{
                        script{
                            withDockerRegistry(credentialsId: 'ef02a1f0-d2bd-4723-b15a-a380ee0a8502', url: ''){
    
                            sh"docker push ${REPOSITORY}:V0.0.${BUILD_NUMBER} "
                            
                            }
                        }
                    }
                }
                stage("Deployment"){
                    steps{
                       script{
                        sh """
                            sed -i 's|image: .*|image: ${REPOSITORY}:V0.0.${BUILD_NUMBER}|' deployment.yaml

                            cat deployment.yaml
                        """
                        withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'minikube', contextName: '', credentialsId: 'k8-secret-token', namespace: '', serverUrl: 'https://127.0.0.1:57833']]) {
                                sh"kubectl apply -f deployment.yaml"
                        
                        }
                       }
                    }   
                }
                
    }
    post{
        failure{
            echo "Deployment failed, removing recent build docker image"

            withDockerRegistry(credentialsId: 'ef02a1f0-d2bd-4723-b15a-a380ee0a8502', url: ''){
    
                sh"docker rmi ${REPOSITORY}:V0.0.${BUILD_NUMBER} "
                            
            }
            cleanWs()
            }
        success{
            echo "deployment completed"
            cleanWs()
        }
    }
}

