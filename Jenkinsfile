pipeline {
    agent any

    stages{

        stage("fazendo um echo"){
            steps{

                script{
                    dockerapp = docker.build("christiannd/kubenews:${env.BUILD_ID}","./src")
                }

            }
        }

    }

}
