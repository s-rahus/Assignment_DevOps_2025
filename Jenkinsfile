pipeline {
    agent any  // Runs the pipeline on any available agent

    environment {
        // Define environment-specific variables
        BUILD_ENV = 'development'
        TEST_ENV = 'test'
        PROD_ENV = 'production'
    }

    tools {
        jdk 'JDK21'  // Ensure JDK 11 (or your preferred JDK) is configured in Jenkins
        maven 'Maven 3.9.9'  // Ensure Maven is configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from version control
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Set build environment to 'build'
                script {
                    env.BUILD_ENV = 'build'
                }
                // Maven clean install to build the project
                sh 'mvn clean install -Denv=${BUILD_ENV}'
            }
        }

        stage('Test') {
            steps {
                // Set test environment to 'test'
                script {
                    env.TEST_ENV = 'test'
                }
                // Run tests using Maven
                sh 'mvn test -Denv=${TEST_ENV}'
            }
        }

        stage('Deploy to Staging') {
            when {
                branch 'develop'  // This stage only runs for the develop branch
            }
            steps {
                // Deploy to staging (this could be any deployment mechanism)
                script {
                    echo "Deploying to Staging Environment"
                }
            }
        }

        stage('Deploy to Prod') {
            when {
                branch 'master'  // This stage only runs for the master branch
            }
            steps {
                // Set prod environment to 'prod'
                script {
                    env.PROD_ENV = 'prod'
                }
                // Deploy to production (this could be any deployment mechanism)
                script {
                    echo "Deploying to Production Environment"
                }
            }
        }
    }

    post {
        always {
            // Always run cleanup or archiving steps
            echo 'Cleaning up after build...'
        }

        success {
            // Actions to take when the build is successful
            echo 'Build, test, and deployment succeeded!'
        }

        failure {
            // Actions to take when the build fails
            echo 'Build or tests failed!'
        }
    }
}