pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
    }

    stages {
        stage('GIT') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/hwafa/timesheetproject.git'
            }
        }

        stage('Maven Build') {
            steps {
                sh "mvn clean install -DskipTests"
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t abderrahmen2002/abderrahmen2002/alpine:v1 ."
            }
        }

        stage('Docker Push') {
            steps {
                sh "echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin"
                sh "docker push abderrahmen2002/abderrahmen2002/alpine:v1"
            }
        }
    }
}
