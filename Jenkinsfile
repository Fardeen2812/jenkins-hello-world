pipeline {
    agent any
    
    tools {
        maven "M398"
    }

    stages {

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
                junit(testResults: 'target/surefire-reports/Test-*.xml',keepProperties: true,keepTestNames: true)
            }
        }

        stage('Containarization ') {
            steps {
                script {
                    // Archive test results
                    sh 'echo Docker build Image'
                    sh 'echo Docker Tag Image'
                    sh 'echo Docker Push Image'
                }
            }
        }
    }

    stage('Kubernetes Deployment'){
        steps {
            sh 'echo Deploy to kubernates using ArgoCD'
            
        }
    }

    Stage('Integration Testing') {
        steps {
            sh "sleep 10s"
            sh 'echo Testing using Curl commands...'
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
