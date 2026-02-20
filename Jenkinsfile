pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "yourdockerhub/demo-app"
    }

    stages {

        stage('Check Java') {
            steps {
                sh 'java -version'
                sh 'mvn -version'
                sh 'echo $JAVA_HOME'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    passwordVariable: 'PASS',
                    usernameVariable: 'USER'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f demo-app || true
                docker run -d --name demo-app -p 8080:8080 $DOCKER_IMAGE
                '''
            }
        }
    }
}
