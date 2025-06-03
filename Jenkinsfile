pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Docker Hub credentials
        GIT_CREDENTIALS = credentials('github-creds')     // GitHub username and PAT
        IMAGE_NAME = "santhoshathili/sampleapi"
    }

    stages {
        stage('Clone') {
            steps {
                script {
                    sh 'rm -rf SampleJenkinsCiCdAPI' // Clean up before cloning
                    sh """
                    git config --global credential.helper store
                    git clone https://${GIT_CREDENTIALS_USR}:${GIT_CREDENTIALS_PSW}@github.com/SanthoshAthili3101/SampleJenkinsCiCdAPI.git
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}", "SampleJenkinsCiCdAPI/")
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh 'docker rm -f sampleapi-container || true'
                    sh "docker run -d -p 8081:8080 --name sampleapi-container ${IMAGE_NAME}"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        docker.image("${IMAGE_NAME}").push()
                    }
                }
            }
        }
    }
}
