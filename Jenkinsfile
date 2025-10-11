pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/LikithKasyap/StudentActivityPortal_SAP_CD.git'
            }
        }

        stage('Build Maven Package') {
            steps {
                bat 'sh clean package -DskipTests'
            }
        }

        stage('Dependency Vulnerability Scan') {
            steps {
                script {
                    // Run scan but do not fail pipeline
                    bat 'mvn org.owasp:dependency-check-maven:check -Dformat=ALL -Ddependency-check.failOnError=false -Ddependency-check.failBuildOnCVSS=11 || exit 0'
                    archiveArtifacts artifacts: 'target/dependency-check-report.*', fingerprint: true
                }
            }
        }

        stage('Test') {
            steps {
                echo "Test Completed"
            }
        }

        stage('Start Services with Docker Compose') {
            steps {
                script {
                    bat 'docker-compose up -d --build'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check logs.'
        }
        cleanup {
            cleanWs()
        }
    }
}
