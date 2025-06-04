pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1" // Replace with your region
        ECR_REPO_NAME = "votingapi"
        AWS_ACCOUNT_ID = "982081054052" // Replace with your AWS account ID
        IMAGE_TAG = "latest"
        IMAGE_NAME = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}"
        AWS_CREDS = credentials('aws-ecr-creds')
    }

    stages {
        stage('Clone') {
            steps {
                git credentialsId: 'github-creds', url: 'https://github.com/SanthoshAthili3101/SampleJenkinsCiCdAPI.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}", ".")
                }
            }
        }

        stage('Authenticate to ECR') {
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
