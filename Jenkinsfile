pipeline {
    agent any

    stages {
        stage('BUILD DOCKER IMAGE') {
            steps {
                echo 'Building docker image'
                sh 'docker build -t sabirsaheel0/jenkinstest:${BUILD_NUMBER} .'
            }
        }
        stage('BUILD LOGIN + PUSH') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: 'uname')]) {
                    sh 'echo ${pass} | docker login -u ${uname} --password-stdin'
                }
                sh 'docker push sabirsaheel0/jenkinstest:${BUILD_NUMBER}'
                sh 'docker logout'
            }
        }
        stage('Clean Docker Images'){
            steps{
                sh 'docker rmi sabirsaheel0/jenkinstest:${BUILD_NUMBER}'
            }
        }
    }
    
}