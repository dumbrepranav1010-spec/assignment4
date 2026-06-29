pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'dumbrepranav10'
        IMAGE_NAME     = 'ass4'
        IMAGE_TAG      = 'v1'
        GIT_REPO_URL   = "https://github.com/dumbrepranav1010-spec/assignment4.git"
	DOCKER_CRED	= credentials('dockerhub')
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: '${GIT_REPO_URL}'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                    sh 'echo $DOCKER_CRED_PSW | docker login -u $DOCKER_CRED_USR --password-stdin'
                    sh 'docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}'
                    sh 'docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:latest'
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh 'kubectl set image deployment/assign4 ass4=${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}'
                
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
