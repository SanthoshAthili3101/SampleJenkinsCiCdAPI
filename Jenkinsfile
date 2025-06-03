pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/SanthoshAthili3101/SampleJenkinsCiCdAPI.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('sampleapi-image')
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Stop previous container
                    sh 'docker rm -f sampleapi-container || true'
                    // Run new container
                    sh 'docker run -d -p 8081:80 --name sampleapi-container sampleapi-image'
                }
            }
        }
    }
}
