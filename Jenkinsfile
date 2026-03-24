pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage('clean_package') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('build_img') {
            steps {
                sh 'docker build -t pavan1img .'
            }
        }

        stage('tag_img') {
            steps {
                sh 'docker tag pavan1img pavansai33/pavan1img:v123'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('docker_push') {
            steps {
                sh 'docker push pavansai33/pavan1img:v123'
            }
        }

        stage('k8s_dep') {
            steps {
                sh 'kubectl apply -f manifestfile.yml'
            }
        }
    }
}
