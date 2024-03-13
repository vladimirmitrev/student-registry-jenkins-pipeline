pipeline {
    agent any
    stages {
        stage("Repo checkout") {
            steps {
                checkout scm
            }
        }
        stage('NPM install') {
            steps{
                bat "npm install"
            }
        }
        stage('Run integration tests') {
            steps{
                bat "npm run test"
            }
        }
        stage('Build Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: '2aeb69d4-2458-4df1-9351-6cecfc8b797f', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    bat """docker build -t orion87/student-registry:1.0.0 .
                        docker login -u %DOCKER_USERNAME% --password %DOCKER_PASSWORD%
                        docker push orion87/student-registry:1.0.0"""
                }
            }
        }
        stage('Deploy Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: '2aeb69d4-2458-4df1-9351-6cecfc8b797f', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    bat """docker pull orion87/student-registry:1.0.0
                            docker-compose -f docker-compose.yml up -d"""
                }
            }
        }
    }
}