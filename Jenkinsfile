pipeline {
    agent any

    environment {
        APP_NAME = 'devops-demo'
        IMAGE = "localhost/devops-demo:${BUILD_NUMBER}"
        CONTAINER = 'devops-demo'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'python3 -m pip install -r requirements.txt'
            }
        }

        stage('Build Application') {
            steps {
                sh 'echo "Application build successful"'
            }
        }

        stage('Build Image - Rootless') {
            steps {
                sh '''
                    podman build \
                      -t ${IMAGE} \
                      .
                '''
            }
        }

        stage('Test Image - Rootless') {
            steps {
                sh '''
                    podman run -d \
                      --name ${CONTAINER}-test \
                      -p 5002:5000 \
                      ${IMAGE}
                '''

                sh '''
                    sleep 5
                    curl -f http://127.0.0.1:5002
                '''

                sh '''
                    podman stop ${CONTAINER}-test || true
                    podman rm ${CONTAINER}-test || true
                '''
            }
        }

        stage('Deploy - Rootful') {
            steps {
                sh '''
                    sudo podman pull ${IMAGE} || true

                    sudo podman stop ${CONTAINER} || true
                    sudo podman rm ${CONTAINER} || true

                    sudo podman run -d \
                      --name ${CONTAINER} \
                      -p 5000:5000 \
                      ${IMAGE}
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    sleep 5
                    curl -f http://127.0.0.1:5000
                '''
            }
        }
    }
}
