pipeline {
    agent any  // Runs the pipeline on any available agent

    tools {
        jdk 'JDK21'  // Ensure you have JDK 11 (or your preferred JDK) configured in Jenkins
        maven 'Maven'  // This assumes you've configured Maven 3.6.3 in Jenkins (Configure in "Manage Jenkins > Global Tool Configuration")
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
            }
        }

        stage('Test') {
            steps {
                // Run the tests using Maven (assuming you have tests configured)
                bat 'mvn test'
            }
        }

        //stage('Run') {
          //  steps {
                // You can run the application if desired, for example:
                //bat 'java -jar target/my-application.jar'  // Replace with your actual JAR file path
            //}
        //}
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