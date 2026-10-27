pipeline {
    agent any

    tools {
        maven 'M2_HOME'
    }

    stages {
        stage('Checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/RaedAffes/country-service.git'
            }
        }

        stage('Build maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Dockerfile') {
            steps {
                sh 'docker build . -t raedaffes/country-service-my-country-service2:$BUILD_NUMBER'

                withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                    sh 'echo "$dockerhubpwd" | docker login -u raedaffes --password-stdin'
                    sh 'docker push raedaffes/country-service-my-country-service2:$BUILD_NUMBER'
                }
            }
        }

        stage('Deploy to kubernetes') {
            steps {
                script {
                    kubeconfig(credentialsId: 'kubeconfig-file', serverUrl: '') {
                        sh 'kubectl apply -f deployment.yaml'
                        sh 'kubectl apply -f service.yaml'
                        sh 'kubectl set image deployment/country-service country-service=raedaffes/country-service-my-country-service2:$BUILD_NUMBER'
                        sh 'kubectl rollout status deployment/country-service'
                    }
                }
            }
        }
    }
}

