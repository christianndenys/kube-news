pipeline {
    agent any

    stages{

        stage("Build da Imagem"){
            steps{

                script{
                    dockerapp = docker.build("christiannd/kubenews:v${env.BUILD_ID}","./src")
                }

            }
        }

        stage("Push da Imagem"){
            steps{

                script{
                    docker.withRegistry('https://registry.hub.docker.com','dockerhub'){
                        dockerapp.push('latest')
                        dockerapp.push("v${env.BUILD_ID}")
                    }
                }

            }
        }

        stage("Deploy no k8s"){
            environment {
                tag_version = "v${env.BUILD_ID}"
            }
            steps{
                withKubeConfig ([credentialsId: 'kubeconfig']){
                    sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }

            }
        }

    }

}
