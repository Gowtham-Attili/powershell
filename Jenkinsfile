pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = 'df54a111-3c2f-4ba3-a6d5-306a5e38cdfd' // Replace with your Docker Hub credentials ID in Jenkins
        DOCKER_REPO = "gowtham47/newimage" // Replace with your Docker Hub username and image name
        DOCKER_TAG = "latest" // Replace with the desired image tag
        GIT_REPO_URL = "https://github.com/Gowtham-Attili/sim.git" // Replace with your GitHub repository URL
        DOCKERFILE_PATH = "Dockerfile" // Replace with the path to your Dockerfile in the repository
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: GIT_REPO_URL
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage // Define dockerImage variable in this scope
                    dockerImage = docker.build("${DOCKER_REPO}:${DOCKER_TAG}", "-f ${DOCKERFILE_PATH} .")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    def dockerImage // Define dockerImage variable in this scope
                    dockerImage = docker.image("${DOCKER_REPO}:${DOCKER_TAG}") // Retrieve the existing Docker image
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        dockerImage.push()
                    }
                }
            }
        }
  	    stage('List pods') {
  	        steps{
                withKubeConfig([credentialsId: 'k8s-id']) {
                //sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'  
               // sh 'chmod u+x ./kubectl'  
                    //sh './kube.ps1'
                    //pwsh -command "kube.ps1"
                    powershell script: "./kube.ps1"
  	            }
            }
  	    }

}
}
