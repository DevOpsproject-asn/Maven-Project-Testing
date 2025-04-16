pipeline {

    agent any

    tools {
        maven 'maven3.8.2'
    }

    triggers {
        pollSCM('* * * * *')
    }

    options {
        timestamps()
        buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
    }

    environment {
        AWS_REGION = 'ap-south-1'
        ECR_REPO = '952149495092.dkr.ecr.ap-south-1.amazonaws.com/maventestproject'
        IMAGE_NAME = 'maventestproject'
    }

    stages {

        stage('CheckOutCode') {
            steps {
                git branch: 'main', credentialsId: 'githubcred', url: 'https://github.com/DevOpsproject-asn/Maven-Project-Testing.git'
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean package"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${ECR_REPO} ."
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                withCredentials([string(credentialsId: 'AWS_ECR_CREDENTIALS', variable: 'AWS_CREDS')]) {
                    sh '''
                        export ${AWS_CREDS}
                        aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPO
                        docker push $ECR_REPO
                    '''
                }
            }
        }
    }
}
