pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK17'
    }

    environment {
        DOCKER_IMAGE = "yourdockerhub/demo-app"
    }

    stages {

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
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'docker login -u $USER -p $PASS'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 8080:8080 $DOCKER_I_
