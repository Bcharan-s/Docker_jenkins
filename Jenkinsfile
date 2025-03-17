pipeline{
    agent any 

    stages{
        stage("SCm Checkout"){
           steps{
            git credentialsId: 'ed4dd9ce-380a-44c7-a68b-e923954ceb9c', url: 'https://github.com/Bcharan-s/Docker_jenkins.git', branch: 'main'
            cleanWs()
        
        }
    }

     stage("Docker Build Image"){
        steps{
            script{
                sh "docker build -t ${env.JOB_NAME}:latest ."
            }
        }
     }
    stage("Docker Push Image"){
        steps{
            script{
            withDockerRegistry(credentialsId: '6c223acd-8fe3-41b8-878c-a8775b2d83df', url: 'https://index.docker.io/v1/') {
    // some block
               sh"docker push ${env.JOB_NAME}:latest "
            }
}
        }
    }
}
}
