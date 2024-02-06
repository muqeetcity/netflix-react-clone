pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("905418448047.dkr.ecr.us-east-1.amazonaws.com/netflix-jan:latest:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 905418448047.dkr.ecr.us-east-1.amazonaws.com/netflix-jan:latest:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('905418448047.dkr.ecr.us-east-1.amazonaws.com/netflix-jan:latest', 'ecr:us-east-1:dyan-ecr') {
                    // build image
                    def myImage = docker.build("905418448047.dkr.ecr.us-east-1.amazonaws.com/netflix-jan:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
