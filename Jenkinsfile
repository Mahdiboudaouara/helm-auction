pipeline {
    agent any
    environment {
        SERVER_ADDRESS = '3.123.129.244'
        SERVER_USERNAME = 'ec2-user'
        DOCKER_IMAGE_NAME = 'mahdiboudaouara/reactappimage'
        PROJECT_NAME = 'client'
        REPO_SERVER = '739761511001.dkr.ecr.eu-central-1.amazonaws.com'
        ECR_REGISTRY = '739761511001.dkr.ecr.eu-central-1.amazonaws.com/ecr-mahdi'
        APP_URL = '139-144-162-115.ip.linodeusercontent.com'
        AWS_REGION = 'eu-central-1'
    }
    stages {
        stage('Push Image') {
            steps {
                script {
                    withKubeConfig([credentialsId: 'clusterkubeconfig', serverUrl: 'https://c81ac799-c9ef-4da4-9d8a-872d8e6400c8.eu-central-2.linodelke.net']) {
                        // sh 'envsubst < kubernetes/deployment.yaml | kubectl apply -f -'
                        echo "IMAGE_TAG value is: ${params.IMAGE_TAG}"
                        sh 'helmfile sync'
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
