pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/cikkasinha/python-jenkins-demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip3 install --break-system-packages -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-jenkins-demo .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker tag flask-jenkins-demo $DOCKER_USER/flask-jenkins-demo
                      docker push $DOCKER_USER/flask-jenkins-demo
                    '''
                }
            }
        }
    }
}

