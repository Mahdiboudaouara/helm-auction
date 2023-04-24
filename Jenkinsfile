pipeline {
    agent any
    environment {
        appName= ${params.IMAGE_TAG}
        dockerImageName= ${params.IMAGE_TAG}
        imageTag= ${params.IMAGE_TAG}
    }
    stages {
        stage('Push Image') {
            steps {
                script {
                    withKubeConfig([credentialsId: 'clusterkubeconfig', serverUrl: 'https://c81ac799-c9ef-4da4-9d8a-872d8e6400c8.eu-central-2.linodelke.net']) {
                        // sh 'envsubst < kubernetes/deployment.yaml | kubectl apply -f -'
                        sh 'envsubst < helmfile.yaml | helmfile sync'
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
