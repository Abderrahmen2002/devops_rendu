pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('Dockerhub_pwd')
    }

    stages {
        stage('GIT') {
            steps {
                git branch: 'master', 
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
                sh "docker build -t <dockerhub-username>/<repo>:v1 ."
            }
        }

        stage('Docker Push') {
            steps {
                sh "echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin"
                sh "docker push <dockerhub-username>/<repo>:v1"
            }
        }
    }
}
