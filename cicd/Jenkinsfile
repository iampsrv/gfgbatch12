pipeline {
    agent {label 'slave'}


    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/iampsrv/batchsevenproject.git'
                sh "ls -ltr"
            }
        }
        stage('Setup') {
            steps {
                sh "pip install -r requirements.txt"
            }
        }
        stage('Test') {
            steps {
                sh "pytest test.py"
            }
        }
        stage('Build Docker Image')
        {
            steps
            {
                sh 'sudo -S docker build -t psrv3/batchsevengithub-action .'
                echo "Docker image build successfully"
                sh "docker images"
            }
        }
        stage('Push Docker Image')
        {
            steps
            {
                sh 'sudo -S docker push psrv3/batchsevengithub-action'
                echo "Docker image push successfully"
            }
        }
        stage('Deploy new Docker Image')
        {
            steps
            {   sh 'sudo -S docker rm -f flaskapp'
                sh 'sudo -S docker rmi psrv3/jenkins-flask'
                sh 'sudo -S docker run --name flaskapp -itd -p 5001:5001 psrv3/jenkins-flask'
                echo "Docker image push successfully"
            }
        }
    }
}
