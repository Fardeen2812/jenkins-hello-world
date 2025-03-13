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
                sh "echo Sleep-time - ${params.SLEEP_TIME}, port - ${params.APP_PORT}, Branch - ${param.BRANCH_NAME}"
            
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

    stage('Local Deployment'){
        steps {
            sh """ java -jar target/hello-demo-*.jar > /dev/null & """
        }
    }

    stage('Integration Testing') {
        steps {
            sh 'sleep "${params.SLEEP_TIME}"'
            sh 'curl -s http://localhost:$"${params.APP_PORT}"/hello'
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
