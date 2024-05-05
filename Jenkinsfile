pipeline {
    agent any
    environment{
        M2_HOME="/opt/maven"
        DOCKER_HUB_CREDENTIALS = 'dockerhub'
        DOCKER_REGISTRY = 'docker.io'
    }

    stages {
        stage('Test'){
            steps{
                sh "echo hello World"
            }
        }

        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/gangadhar-aws/02-Maven-Webapp.git'

                // Build code using Maven
                sh "${M2_HOME}/bin/mvn clean package"

            }
        }

        stage('Build Docker Image'){
            steps{
                script{
                    sh 'docker build -t my_webapp:latest .'
                    sh 'docker tag my_webapp:latest my_webapp:$BUILD_NUMBER'
                }

            }
        }
        stage('Docker Login'){
            steps{
                script {
                    docker.withRegistry("${DOCKER_REGISTRY}", "${DOCKER_HUB_CREDENTIALS}") 
                    // {
                    //     docker.image("${DOCKER_IMAGE_NAME}").push("${env.BUILD_NUMBER}")
                    // }
                }
             }
        }


        // stage('Publish Image DH'){
        //     steps{ 
        //         withDockerRegistry([ credentialsId: "DOCKERHUB", url: "https://hub.docker.com/u/gangadharbsk" ])
        //         sh 'docker push gangadharbsk/my_webapp:latest'
        //     }
        // }
    }
}