pipeline {
    agent any

    tools {
        sonarRunner 'sonar-scanner'
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/ahmadali-114/Practice-Project.git'
            }
        }

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh """
                    sonar-scanner \
                    -Dsonar.projectKey=practice-project \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://10.0.2.15:9000
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t practice-html-site .'
            }
        }

        stage('Deploy Website') {
            steps {
                sh '''
                docker stop html-site || true
                docker rm html-site || true
                docker run -d -p 8085:80 --name html-site practice-html-site
                '''
            }
        }
    }
}
