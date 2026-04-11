pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "vaishnavijp/my-app"
        DOCKER_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/vaishjp/finacplus-cicd-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-pass',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $DOCKER_IMAGE:$DOCKER_TAG
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                minikube kubectl -- set image deployment/my-app nginx=$DOCKER_IMAGE:$DOCKER_TAG 
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment Successful "
        }
        failure {
            echo "Pipeline Failed "
        }
    }
}
