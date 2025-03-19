pipeline{
    agent any 
    environment{
        REPOSITORY = "cherry98/first_app"
        K8_SECRET_TOKEN = Credentials('k8-secret-token')
    }

    stages{
        stage("Scm Checkout"){
           steps{
                git credentialsId: 'ed4dd9ce-380a-44c7-a68b-e923954ceb9c', url: 'https://github.com/Bcharan-s/Docker_jenkins.git', branch: 'main'
            
        
            }
        }


                stage("Docker Build Image"){
                    steps{
                        script{
                            sh "docker build -t ${REPOSITORY}:V0.0.${BUILD_NUMBER} ."
                            echo "IMAGE BUILD SUCCESSFUL"

                        }
        
                    }
                }
                stage("Docker Push Image"){
                    steps{
                        script{
                            withDockerRegistry(credentialsId: '6c223acd-8fe3-41b8-878c-a8775b2d83df', url: ''){
    
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

                            kubectl apply -f deployment.yaml --token=${K8_SECRET_TOKEN}
                        """
                       }
                    }   
                }
                
    }
    post{
        failure{
            echo "Deployment failed"
            cleanWs()
        }
        success{
            echo "deployment completed"
            cleanWs()
        }
    }
}