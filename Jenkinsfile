pipeline{
    agent any

    environment {
        DOCKERHUB_USERNAME = "cbastias"
        APP_NAME = "gitops-argo-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = "dockerhub"
    }
    stages{

        stage('Clenup workspace'){

            steps{
                script{
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM'){

            steps{
                script{
                    git credentialsId: 'github',
                    url: 'https://github.com/cbastias/gitops_argocd_project.git',
                    branch: 'main'

                }
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('',REGISTRY_CREDS){
                    docker_image.push("$BUILD_NUMBER")
                    docker_image.push('latest')
                    }
                }
            }
        }
        stage('Delete Docker images')
        {
            steps{
                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Trigger config change pipeline')
        {
            steps{
                script{
                    sh "curl -v -k -user cbastias:11157101ef747747f9625244461ca2178b -X POST -H 'cache-control: no cache'  -H 'content-type: application/x-www-form-urlencoded' -data 'IMAGE_TAG=${IMAGE_TAG}' 'http://54.80.134.199:8080/job/gitops-argocd_CD//buildWithParameters?token=gitops-config'"
                }
            }
        }
    }
}
