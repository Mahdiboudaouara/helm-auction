pipeline {
    agent any
    stages {
        stage('Push Image') {
            steps {
                script {
                    withKubeConfig([credentialsId: 'clusterkubeconfig', serverUrl: 'https://c81ac799-c9ef-4da4-9d8a-872d8e6400c8.eu-central-2.linodelke.net']) {
                        // sh 'envsubst < kubernetes/deployment.yaml | kubectl apply -f -'
                        sh 'helm upgrade --install helm stable/helm'
                        sh 'helm plugin install https://github.com/databus23/helm-diff --version v3.7.0'
                        sh "helmfile diff --set appName=${params.PROJECT_NAME} --set dockerImageName=${params.DOCKER_IMAGE_NAME} --set imageTag=${params.IMAGE_TAG}"
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
