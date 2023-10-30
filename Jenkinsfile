pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Get the current branch name
                    currentBranch = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()

                    // Get the current repository URL
                    currentRepoUrl = env.BUILD_URL // You can use a different environment variable for the repository URL

                    if (currentBranch == 'main' && currentRepoUrl == 'https://github.com/MaryPhani/bootcamp.git') {
                        echo 'Building and testing for UAT branch in the specific repository'
                        // Add your build and test commands for UAT branch here
                    } else if (currentBranch == 'test' && currentRepoUrl == 'https://github.com/MaryPhani/spring_1.git') {
                        echo 'Building and testing for Dev branch in the specific repository'
                        // Add your build and test commands for Dev branch here
                    } else {
                        echo 'Skipping build for other branches or repositories'
                    }
                }
            }
        }

        stage('Code Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        /*stage('Compile and Run Sonar Analysis') {
            steps {
                sh "mvn clean verify sonar:sonar  \
            -Dsonar.projectKey=BP-sonarqube \
            -Dsonar.host.url=http://182.18.184.81:9000/ \
            -Dsonar.login=sqp_684c8264f22b0aba50b8c347a0b70d2f7258805e"
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

        stage('Snyk Test') {
            steps {
                echo 'Snyk Testing...'
                snykSecurity (
                    projectName: 'Snyk_security_tool', 
                    snykInstallation: 'snyk@latest', 
                    snykTokenId: 'snyk_token',
                    failOnIssues: false
                )
            }
        } */

 

        stage('Build Image') {
            steps {
                sh 'docker build -t frontendapp-${app} .'
            }
        }
        stage('Push to ECR') {
            steps {
                sh 'aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 490167669940.dkr.ecr.ap-southeast-1.amazonaws.com'
                sh "docker tag frontendapp-${app}:latest  490167669940.dkr.ecr.ap-southeast-1.amazonaws.com/eks-frontend-app-deployment:${app}-${commitId}"
                sh "docker push 490167669940.dkr.ecr.ap-southeast-1.amazonaws.com/eks-frontend-app-deployment:${app}-${commitId}"
            } 
        } 
        /*stage('Deploying ECR Image to EKS') {
            steps {
                script {
                    sh '''
                        aws eks update-kubeconfig --name $EKS_CLUSTER_NAME
                        kubectl apply -f eks-deploy-k8s.yaml

                    '''
                }
            }
        } */
    }
}
