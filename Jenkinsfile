pipeline {
    agent any

    parameters {
        string(name: 'REPO_URL', defaultValue: 'https://github.com/vaishjp/finacplus-cicd-pipeline.git', description: 'Git Repository URL')
        string(name: 'DOCKER_IMAGE', defaultValue: 'vaishnavijp/my-app', description: 'Docker Image Name')
        string(name: 'DEPLOYMENT_NAME', defaultValue: 'my-app', description: 'Kubernetes Deployment Name')
    }

    environment {
        DOCKER_IMAGE = "${params.DOCKER_IMAGE}"
        DOCKER_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: "${params.REPO_URL}"
            }
        }

        stage('Test Application') {
            steps {
                sh '''
                echo "Running test..."
                node app.js > app.log 2>&1 &
                APP_PID=$!
                sleep 5

                RESPONSE=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:3000 || true)

                if [ "$RESPONSE" -eq 200 ]; then
                    echo "Test Passed"
                else
                    echo "Test Failed"
                    cat app.log
                    kill $APP_PID
                    exit 1
                fi

                kill $APP_PID
                '''
            }
        }

        stage('Logging Info') {
            steps {
                sh '''
                echo "Build Number: $BUILD_NUMBER"
                echo "Docker Image: $DOCKER_IMAGE:$DOCKER_TAG"
                '''
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
                sh """
                minikube kubectl -- set image deployment/${params.DEPLOYMENT_NAME} nginx=${DOCKER_IMAGE}:${DOCKER_TAG}
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                sh """
                echo "Verifying deployment..."
                minikube kubectl -- rollout status deployment/${params.DEPLOYMENT_NAME}
                """
            }
        }
    }

    post {
        success {
            echo "Deployment Successful"
        }
        failure {
            echo "Pipeline Failed"
        }
    }
}
