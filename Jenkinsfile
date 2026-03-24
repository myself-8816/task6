pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage('checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/myself-8816/task6.git']])
                    
                )
            }
        }

        stage('clean_package') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('build_img') {
            steps {
                sh 'docker build -t pavanimg .'
            }
        }

        stage('tag_img') {
            steps {
                sh 'docker tag pavanimg pavansai33/pavanimg:v123'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docke-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('docker_push') {
            steps {
                sh 'docker push pavansai33/pavanimg:v123'
            }
        }

        stage('k8s_dep') {
            steps {
                sh 'kubectl apply -f manifestfile.yml'
            }
        }
    }
}
