pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1" // Mumbai region
        AWS_ACCOUNT_ID = "982081054052"
        ECR_REPO_NAME = "votingapi"
        IMAGE_TAG = "latest"
        IMAGE_NAME = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}"

        // Jenkins credentials (Type: "Username with password")
        AWS_CREDS = credentials('aws-ecr-creds')
        GIT_CREDENTIALS = credentials('github-creds')
    }

    stages {
        stage('Clean Workspace') {
            steps {
                sh 'rm -rf SampleJenkinsCiCdAPI || true'
            }
        }

        stage('Clone Repository') {
            steps {
                script {
                    sh """
                    git config --global credential.helper store
                    git clone https://${GIT_CREDENTIALS_USR}:${GIT_CREDENTIALS_PSW}@github.com/SanthoshAthili3101/SampleJenkinsCiCdAPI.git
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('SampleJenkinsCiCdAPI') {
                    script {
                        docker.build("${IMAGE_NAME}", ".")
                    }
                }
            }
        }

        stage('Authenticate to AWS ECR') {
            steps {
                script {
                    sh """
                    aws configure set aws_access_key_id ${AWS_CREDS_USR}
                    aws configure set aws_secret_access_key ${AWS_CREDS_PSW}
                    aws configure set region ${AWS_REGION}
                    aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    """
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    sh "docker push ${IMAGE_NAME}"
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
    }
}
