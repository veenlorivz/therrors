pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                sh '''
                docker image rm therrors/therrors-website:v1.($BUILD_ID - 1)
                docker build -t therrors/therrors-website:v1.$BUILD_ID .
                '''
            }
        }
        stage('test'){
            steps{
                echo 'Testing...'
            }
        }
        stage('Release'){
            steps{
                withCredentials([string(credentialsId: 'DOCKER_PASS', variable: 'DOCKER_PASS')]) {
                    sh '''
                    docker login -u therrors -p $DOCKER_PASS
                    docker push therrors/therrors-website:v1.$BUILD_ID
                    '''
                }
            }
        }
        stage('Deploy'){
            steps{
                sh '''
                docker stop therrors-website && docker rm therrors-website
                docker run -d --name therrors-website -p 80:80 therrors/therrors-website:v1.$BUILD_ID
                '''
            }
        }
    }
}
