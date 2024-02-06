pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages {
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("081595744103.dkr.ecr.us-east-2.amazonaws.com/jan-netlix-clone:latest")
                }
            }
        }

        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 081595744103.dkr.ecr.us-east-2.amazonaws.com/jan-netlix-clone:latest'
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    // push image to ECR
                    docker.withRegistry('https://081595744103.dkr.ecr.us-east-2.amazonaws.com', 'ecr:us-east-2:muqeettasawar-ecr') {
                        // tag and push image
                        docker.image("081595744103.dkr.ecr.us-east-2.amazonaws.com/jan-netlix-clone:latest").push()
                    }
                }
            }
        }
    }
}

