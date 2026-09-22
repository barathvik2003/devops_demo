pipeline {
    agent any

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

        stage('Build Container Image') {
            steps {
                sh 'podman build -t localhost/devops-demo:${BUILD_NUMBER} .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    podman stop devops-demo || true
                    podman rm devops-demo || true

                    podman run -d \
                        --name devops-demo \
                        --network podman \
                        -p 5000:5000 \
                        localhost/devops-demo:${BUILD_NUMBER}
                '''
            }
        }
    }
}
