pipeline {
    agent any

    stages{

        stage("Build da Imagem"){
            steps{

                script{
                    dockerapp = docker.build("christiannd/kubenews:${env.BUILD_ID}","./src")
                }

            }
        }

        stage("Push da Imagem"){
            steps{

                script{
                    docker.withRegistry('https://registry.hub.docker.com','dockerhub'){
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }

            }
        }

        stage("deploy no k8s"){
            steps{
                withKubeConfig ([credentialsId: 'kubeconfig']){
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }

            }
        }

    }

}
