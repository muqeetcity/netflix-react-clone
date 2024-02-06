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
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('081595744103.dkr.ecr.us-east-2.amazonaws.com/jan-netlix-clone', 'ecr:us-east-2:muqeettasawar-ecr') {
                    // build image
                    def myImage = docker.build("081595744103.dkr.ecr.us-east-2.amazonaws.com/jan-netlix-clone:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
