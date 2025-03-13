pipeline {
    agent any
    
    tools {
        maven "M398"
    }

    stages {
        stage('Check Maven Version') {
            steps {
                sh 'echo Print Maven version'
                sh 'mvn --version'  // This checks the Maven version on the build agent
            }
        }

        

        stage('Build') {
            steps {
                // Run Maven package
                sh 'mvn clean package -DskipTests=true'
                archiveArtifacts 'target/hello-demo-*.jar'
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
                    junit(testResults: 'target/surefire-reports/*.xml',keepProperties: true,keepTestNames: true)
                }
            }
        }
    }

    stage('Local Deployment'){
        steps {
            sh """ java -jar targets/hello-demo-*.jar > /dev/null & """
        }
    }

    Stage('Integration Testing') {
        steps {
            sh 'sleep 5s'
            sh 'curl -s http://localhost:6767/hello'
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
