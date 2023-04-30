pipeline {
    agent any
    environment {
        APP_URL = '194-195-247-90.ip.linodeusercontent.com'
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
        stage('Replace image tag') {
            steps {
                script {
                    // Check if the values file exists
                    if (fileExists("values/${params.PROJECT_NAME}-values.yaml")) {
                        // Replace the imageTag value in the values file
                        sh "sed -i 's/imageTag:.*/imageTag: ${params.IMAGE_TAG}/' values/${params.PROJECT_NAME}-values.yaml"
                    } else {
                        error "values/${params.PROJECT_NAME}-values.yaml file does not exist"
                    }
                }
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
        stage('commit version update') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        // git config here for the first time run
                        sh 'git config --global user.email "mahdijenkins@jenkins.com"'
                        sh 'git config --global user.name "mahdijenkins"'
                        sh "git remote set-url origin https://${USER}:${PASS}@github.com/Mahdiboudaouara/helm-auction.git"
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh "git push origin HEAD:${BRANCH_NAME}"
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
