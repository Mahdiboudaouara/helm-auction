pipeline {
    agent any
    stages {
        stage('Push Image') {
            steps {
                script {
                    withKubeConfig([credentialsId: 'clusterkubeconfig', serverUrl: 'https://c81ac799-c9ef-4da4-9d8a-872d8e6400c8.eu-central-2.linodelke.net']) {
                        // sh 'envsubst < kubernetes/deployment.yaml | kubectl apply -f -'
                        sh "helmfile sync --set appName=${params.PROJECT_NAME} --set dockerImageName=${params.DOCKER_IMAGE_NAME} --set imageTag=${params.IMAGE_TAG}"
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
