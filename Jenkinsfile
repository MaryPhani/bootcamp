pipeline {
    agent any
    environment {
        app = 'frontend'
        IMAGE_TAG = "frontend-${BUILD_NUMBER}"
        AWS_ACCESS_KEY_ID = credentials('AWS-Creds')
        AWS_SECRET_ACCESS_KEY = credentials('AWS-Creds')
        AWS_DEFAULT_REGION = 'AWS-Region'
        SONAR_LOGIN = credentials('Sonar-Creds')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: "origin/${env.BRANCH_NAME}"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/babu517/springboot-app-deployment-K8S']]])
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Compile and Run Sonar Analysis') {
            steps {
                sh "mvn clean verify sonar:sonar  \
            -Dsonar.projectKey=frontend \
            -Dsonar.host.url=http://18.136.72.199:9000/ \
            -Dsonar.login=sqa_50885d4939be4dfc296b4d5af73a7c307287fec8"
            }
        }
        stage('Push to S3') {
            steps {
                sh 'aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID'
                sh 'aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY'
                sh 'aws configure set default.region $AWS_DEFAULT_REGION'
                sh 'aws s3 ls'
                sh 'aws s3 cp target/*.jar s3://eksfrontendapp/'
                }
            }
               
        stage('Build Image') {
            steps {
                sh 'docker build -t frontendapp-${app} .'
            }
        }
        stage('Push to ECR') {
            steps {
                sh 'aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 490167669940.dkr.ecr.ap-southeast-1.amazonaws.com'
                sh "docker tag frontendapp-${app}:latest  490167669940.dkr.ecr.ap-southeast-1.amazonaws.com/eks-frontend-app-deployment:${app}-${BUILD_NUMBER}"
                sh "docker push 490167669940.dkr.ecr.ap-southeast-1.amazonaws.com/eks-frontend-app-deployment:${app}-${BUILD_NUMBER}"
            }
        }
    }
}
