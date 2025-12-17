pipeline {
    agent any

    environment {
        // We use the Jenkins Build ID to create a unique version for every commit
        IMAGE_TAG = "v${BUILD_NUMBER}" 
        DOCKER_IMAGE = "abderrahmen2002/alpine"
        DOCKER_HUB_CREDENTIALS = credentials('docker_id')
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
                sh "mvn clean install -Dmaven.test.skip=true"
            }
        }

        stage('Docker Build') {
            steps {
                // Build with the unique tag
                sh "docker build -t $DOCKER_IMAGE:$IMAGE_TAG ."
            }
        }

        stage('Docker Push') {
            steps {
                sh "echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin"
                sh "docker push $DOCKER_IMAGE:$IMAGE_TAG"
            }
        }

        stage('Deploy to K8s') {
            steps {
                script {
                    // 1. Update the Deployment to use the new image version
                    sh "kubectl set image deployment/spring-app spring-app=$DOCKER_IMAGE:$IMAGE_TAG -n devops"
                    
                    // 2. Force a restart to ensure it pulls the new code immediately
                    sh "kubectl rollout restart deployment/spring-app -n devops"
                    
                    // 3. Verify the deployment status
                    sh "kubectl rollout status deployment/spring-app -n devops"
                }
            }
        }
    }
}
