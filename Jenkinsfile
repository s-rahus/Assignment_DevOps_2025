pipeline {
    agent any  // Runs the pipeline on any available agent

    tools {
        jdk 'JDK21'  // Ensure you have JDK 11 (or your preferred JDK) configured in Jenkins
        maven 'Maven'  // This assumes you've configured Maven 3.6.3 in Jenkins (Configure in "Manage Jenkins > Global Tool Configuration")
    }


	environment {
        
        STAGING_ENV = 'staging'
        PROD_ENV = 'production'
        APP_NAME = 'my-app-1.0-SNAPSHOT'  // Update this with your app's name
        JAR_PATH = 'target\\my-app-1.0-SNAPSHOT.jar'  // Path to the .jar file
    }
    
    
    stages {
        stage('Checkout') {
            steps {
                // This step checks out the code from the repository (e.g., Git)
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Clean and build the project using Maven
                bat 'mvn clean install'
                bat 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                // Run the tests using Maven (assuming you have tests configured)
                bat 'mvn test'
            }
        }

        stage('Run') {
            steps {
                 //You can run the application if desired, for example:
                bat 'java -jar target/my-app-1.0-SNAPSHOT.jar'  // Replace with your actual JAR file path
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo "Deploying to ${env.STAGING_ENV} environment..."
                // Simulate deploying to staging environment by copying .jar to the staging directory
                bat "if not exist environments\\staging mkdir -p environments\\staging"
                bat "copy ${env.JAR_PATH} environments\\staging\\"
                // Run the app in the staging environment (using different args or settings)
                bat "java -jar environments\\staging\\${env.APP_NAME}.jar --env=staging"
            }
        }

        stage('Deploy to Production') {
            when {
                beforeInput true
                expression {
                    return currentBuild.currentResult == 'SUCCESS'
                }
            }
            input {
                message 'Approve deployment to production?'
                ok 'Deploy'
            }
            steps {
                echo "Deploying to ${env.PROD_ENV} environment..."
                // Simulate deploying to production environment by copying .jar to the production directory
                bat "if not exist environments\\production mkdir -p environments\\production"
                bat "copy ${env.JAR_PATH} environments\\production\\"
                // Run the app in the production environment (using different args or settings)
                bat "java -jar environments\\production\\${env.APP_NAME}.jar --env=production"
            }
        }
        
    }


	
        
        
    post {
        // Post actions such as archiving test results, cleanup, etc.
        //always {
            // Archive the build artifacts and test results (optional)
          //  archiveArtifacts '**/target/*.jar'  // Archive JAR files if you want to keep them
            //junit '**/target/test-*.xml'  // Archive test reports (ensure Maven is configured to generate XML reports)
        //}
       
        success {
            // Actions to take when the build is successful
            echo 'Build and tests passed successfully!'
        }
       
        failure {
            // Actions to take when the build fails
            echo 'Build or tests failed!'
        }
    }
}