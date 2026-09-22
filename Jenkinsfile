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
                sh '''
                    python3 -m pip install -r requirements.txt
                '''
            }
        }
        stage('Build Application') {
            steps {
                sh '''
                    echo "Application build successful"
                '''
            }
        }

        stage('Build Container Image') {
            steps {
                sh '''
                    podman build -t devops-demo:${BUILD_NUMBER} .
                '''
            }
        }

    }
}
