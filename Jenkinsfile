pipeline {
    agent any
    environment {
        IMAGE_NAME = "demo-ci-cd:latest"
    }
    stages {
        stage('Checkout') {
            steps {
                // Uso correcto de checkout con credenciales
                checkout([$class: 'GitSCM', 
                    branches: [[name: 'main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/eybitreh-rgb/springboot-app.git',
                        credentialsId: 'github-token'
                    ]]
                ])
            }
        }
        stage('Build & Test') {
            steps {
                sh 'mvn -B clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t %IMAGE_NAME% .'
            }
        }
        stage('Run Container') {
            steps {
                sh 'docker rm -f demo-ci-cd || true'
                sh 'docker run -d --name demo-ci-cd -p 8080:8080 %IMAGE_NAME%'
            }
        }
    }
    post {
        always {
            junit '**/target/surefire-reports/*.xml'
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}
