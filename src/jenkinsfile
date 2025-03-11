pipeline {
    agent any
    
    tools {
        maven "M398"
    }

    stages {
        stage('Check Maven Version') {
            steps {
                sh 'mvn --version'  // This checks the Maven version on the build agent
            }
        }

        

        stage('Build') {
            steps {
                // Run Maven package
                sh 'mvn clean package -DskipTests=true'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'  // This runs the unit tests
            }
        }

        stage('Post-Build Actions') {
            steps {
                script {
                    // Archive test results
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed. Check the logs for more details.'
        }
    }
}
