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

        // Add more stages as needed for deployment, artifact publishing, etc.
    }
}
    }
}



