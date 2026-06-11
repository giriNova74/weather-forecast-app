pipeline {

    agent any

    environment {

          
       DOCKER_CREDS = credentials('docker-creds')

        IMAGE_NAME = "girinova74/weather-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master', url:'https://github.com/giriNova74/weather-forecast-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
        python3 -m venv venv
        . venv/bin/activate
        pip install -r requirements.txt
        '''

            }
        }

        stage('Run Tests') {
            steps {
                echo 'Skipping tests'
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                docker build \
                -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Docker Push') {
            steps {
                sh '''
                echo $DOCKER_CREDS_PSW | docker login \
                -u $DOCKER_CREDS_USR \
                --password-stdin

                docker push \
                $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
}
