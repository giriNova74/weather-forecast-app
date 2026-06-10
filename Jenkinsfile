pipeline {

    agent any

    environment {

        SONAR_TOKEN = credentials('sonar-token')
        DOCKER_CREDS = credentials('dockerhub-creds')

        IMAGE_NAME = "girinova74/weather-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url:'https://github.com/giriNova74/weather-forecast-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest'
            }
        }

        stage('Sonar Scan') {
            steps {
                sh '''
                sonar-scanner \
                -Dsonar.projectKey=weather-app \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://localhost:9000 \
                -Dsonar.login=$SONAR_TOKEN
                '''
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
