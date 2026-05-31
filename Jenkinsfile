pipeline {

    agent {
        label 'Slave01'
    }

    environment {

        DOCKERHUB_CREDENTIALS = credentials('docker')

        IMAGE_NAME = "umangkhandelwal/aprilsprt2026:v2"
    }

    stages {

        stage('Checkout') {
            steps {

                git branch: 'main',
                url: 'https://github.com/UmangKhandelwal23/PRTMAY2026'
            }
        }

        stage('Build Image') {
            steps {

                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Push Image') {
            steps {

                sh '''
                echo $DOCKERHUB_CREDENTIALS_PSW | docker login \
                -u $DOCKERHUB_CREDENTIALS_USR \
                --password-stdin

                docker push ${IMAGE_NAME}
                '''
            }
        }

        stage('Deploy Container') {
            steps {

                sh '''
                docker stop ecommerce || true
                docker rm ecommerce || true

                docker pull ${IMAGE_NAME}

                docker run -d --name ecommerce -p 80:80 ${IMAGE_NAME}
                '''
            }
        }
    }
}
