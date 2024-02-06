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
                    docker.build("905418448047.dkr.ecr.us-east-1.amazonaws.com/netflix-jan:latest")
                }
            }
        }

        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 905418448047.dkr.ecr.us-east-1.amazonaws.com/netflix-jan:latest'
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    // push image to ECR
                    docker.withRegistry('https://905418448047.dkr.ecr.us-east-1.amazonaws.com/netflix-jan', 'ecr:us-east-1:dyan-ecr') {
                        // tag and push image
                        docker.image("905418448047.dkr.ecr.us-east-1.amazonaws.com/netflix-jan:latest").push()
                    }
                }
            }
        }
    }
}
