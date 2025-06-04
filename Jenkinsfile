pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Docker Hub credentials
        IMAGE_NAME = "santhoshathili/sampleapi"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs() // Clean workspace before starting
            }
        }

        stage('Clone') {
            steps {
                git url: 'https://github.com/SanthoshAthili3101/SampleJenkinsCiCdAPI.git',
                    branch: 'main',
                    credentialsId: 'github-creds'
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('SampleJenkinsCiCdAPI') {
                    script {
                        docker.build("${IMAGE_NAME}")
                    }
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

    post {
        always {
            sh 'docker rm -f sampleapi-container || true' // Clean up container
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}