pipeline{
    agent any 

    stages{
        stage("SCm Checkout"){
           checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'ed4dd9ce-380a-44c7-a68b-e923954ceb9c', url: 'https://github.com/Bcharan-s/Docker_jenkins.git']])
        }
    }
}