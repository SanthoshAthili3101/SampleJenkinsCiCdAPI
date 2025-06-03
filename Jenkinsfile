pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        IMAGE_NAME = "santhoshathili/sampleapi"
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/SanthoshAthili3101/SampleJenkinsCiCdAPI.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}")
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

