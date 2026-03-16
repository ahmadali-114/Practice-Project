pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token')  // Your Jenkins SonarQube token
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Cloning the project from GitHub"
                git branch: 'main', url: 'https://github.com/ahmadali-114/Practice-Project.git'
            }
        }

        stage('SonarQube Scan') {
            steps {
                echo "Running SonarQube analysis"
                sh """
                /opt/sonar-scanner/bin/sonar-scanner \
                -Dsonar.projectKey=practice-project \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://10.0.2.15:9000 \
                -Dsonar.login=${SONAR_TOKEN}
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image"
                sh "docker build -t practice-project:latest ."
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Running Docker container locally"
                sh """
                docker rm -f practice-project || true
                docker run -d --name practice-project -p 8080:80 practice-project:latest
                """
            }
        }

    }

    post {
        always {
            echo "Pipeline finished. Check SonarQube and your app at http://10.0.2.15:8080"
        }
    }
}
