pipeline {
    agent any

    parameters {
    choice(choices: ['main', 'test'], description: 'Select Branch to Build', name: 'BRANCH')
    choice(choices: ['production', 'testing'], description: 'Select Environment', name: 'ENVIRONMENT')

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
    

    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout([$class: 'GitSCM',
                        branches: [[name: "*/${params.BRANCH}"]],
                        userRemoteConfigs: [[url: 'https://github.com/MaryPhani/bootcamp.git']]
                    ])
                }
            }
        }
        
        stage('Build and Test') {
            when {
                expression {
                    params.BRANCH != null && params.ENVIRONMENT != null
                }
            }
            steps {
                script {
                    if (params.BRANCH == 'main') {
                        echo 'Building and testing for main branch'
                        sh 'mvn clean install'
                        // Add commands for the 'main' branch
                    } else if (params.BRANCH == 'test') {
                        echo 'Building and testing for test branch'
                        sh 'mvn clean install'
                        // Add commands for the 'test' branch
                    }
                    
                    if (params.ENVIRONMENT == 'production') {
                        echo 'Using production environment'
                        sh 'mvn clean install'
                        // Commands for the production environment
                    } else if (params.ENVIRONMENT == 'testing') {
                        echo 'Using testing environment'
                        sh 'mvn clean install'
                        // Commands for the testing environment
                    }
                }
            }
        }
        
        stage('Deployment') {
            when {
                expression {
                    params.BRANCH == 'main' && params.ENVIRONMENT == 'production'
                    cp pom.xml /root
                }
            }
            steps {
                echo 'Deploying to production'
                cp pom.xml /
                // Add deployment steps for the main branch and production environment
            }
        }
    }
}
