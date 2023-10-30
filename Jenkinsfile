pipeline {
    agent any
    environment {
        app = 'frontend'
        commitId = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
        IMAGE_TAG = "frontend-${commitId}"
       // AWS_ACCESS_KEY_ID = credentials('AWS-CRED')
       // AWS_SECRET_ACCESS_KEY = credentials('AWS-CRED')
        AWS_DEFAULT_REGION = 'ap-southeast-1'
       // EKS_CLUSTER_NAME = 'sandboxeks1'
        }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: "origin/${env.BRANCH_NAME}"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'gitcred', url: 'https://github.com/MaryPhani/bootcamp.git']]])
            }
        }        
        stage('Build and Test') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'dev') {
                        echo 'Building and testing for Dev branch'
                        sh 'mvn clean install'
                    } else if (env.BRANCH_NAME == 'uat') {
                        echo 'Building and testing for UAT branch'
                        sh 'mvn clean install'
                    } else {
                        echo 'Skipping build for other branches'
                    }
                }
            }
        }
        
        stage('Deployment') {
            when {
                expression {
                    (env.BRANCH_NAME == 'dev' && currentBuild.resultIsBetterOrEqualTo('SUCCESS')) || (env.BRANCH_NAME == 'uat' && currentBuild.resultIsBetterOrEqualTo('SUCCESS'))
                }
            }
            steps {
                script {
                    if (env.BRANCH_NAME == 'dev') {
                        echo 'Deploying to Development environment'
                        
                    } else if (env.BRANCH_NAME == 'uat') {
                        echo 'Deploying to UAT environment'
                        
                    }
                }
            }
        }
    }
}
