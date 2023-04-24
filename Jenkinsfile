pipeline {
    agent any
    environment {
        IMAGE_TAG= "${params.IMAGE_TAG}"
        DOCKER_IMAGE_NAME= "${params.DOCKER_IMAGE_NAME}"
        PROJECT_NAME= "${params.PROJECT_NAME}"
    }
    stages {
        stage('Push Image') {
            steps {
                script {
                    withKubeConfig([credentialsId: 'clusterkubeconfig', serverUrl: 'https://c81ac799-c9ef-4da4-9d8a-872d8e6400c8.eu-central-2.linodelke.net']) {
                        // sh 'envsubst < kubernetes/deployment.yaml | kubectl apply -f -'
                        sh 'helmfile apply --set auctionservice.appName=$PROJECT_NAME auctionservice.dockerImageName=$DOCKER_IMAGE_NAME auctionservice.imageTag=$IMAGE_TAG'
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
