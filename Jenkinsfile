pipeline {
    agent any
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKERHUB_USERNAME ="sabirsaheel0"
        DOCKERHUB_REPO_NAME = "jenkinstest"
    }


    stages {
        stage('BUILD DOCKER IMAGE') {
            steps {
                echo 'Building docker image'
                sh 'docker build -t ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO_NAME}:${IMAGE_TAG} .'
            }
        }
        stage('BUILD LOGIN + PUSH') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: 'uname')]) {
                    sh 'echo ${pass} | docker login -u ${uname} --password-stdin'
                }
                sh 'docker push ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO_NAME}:${IMAGE_TAG}'
                sh 'docker logout'
            }
        }
        stage('Clean Docker Images'){
            steps{
                sh 'docker rmi ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO_NAME}:${IMAGE_TAG}'
            }
        }
        stage('Trigger Next Pipeline') {
            steps{
                build job: 'config-pipeline', parameters: [string(name: 'IMAGE_TAG', value: "${IMAGE_TAG}")]
            }
        }
        stage('CLEANING WORKSPACE') {
            steps{
                script{
                    cleanWs()
                }
            }
        }
    }
    
}