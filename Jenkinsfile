pipeline {
    agent any
    environment {
        APP_URL = '194-195-247-34.ip.linodeusercontent.com'
    }
    stages {
        stage('Setup') {
            steps {
                // Add Bitnami Helm repository and nginx
                sh 'helm repo add bitnami https://charts.bitnami.com/bitnami'
                // Add stable Helm repository
                sh 'helm repo update'
            }
        }
        stage('Push Image') {
            steps {
                script {
                    withKubeConfig([credentialsId: 'clusterkubeconfig', serverUrl: 'https://c81ac799-c9ef-4da4-9d8a-872d8e6400c8.eu-central-2.linodelke.net']) {
                        if (params.PROJECT_NAME && params.DOCKER_IMAGE_NAME && params.IMAGE_TAG) {
                            sh "helmfile --selector app=${params.PROJECT_NAME} sync --set appName=${params.PROJECT_NAME} --set dockerImageName=${params.DOCKER_IMAGE_NAME} --set imageTag=${params.IMAGE_TAG}"
                        } else {
                            sh "helmfile sync"
                        }
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
