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

        stage("Deploy no k8s"){
            environment {
                tag_version = "${env.BUILD_ID}"
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
