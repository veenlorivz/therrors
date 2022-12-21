pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                sh 'docker build -t therrors/therrors-website:v1.$BUILD_ID .'
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
                docker run -d --name therrors-website -p 80:80 therrors/therrors-website:v1.$BUILD_ID
                '''
            }
        }
    }
}
