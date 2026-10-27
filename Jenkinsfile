pipeline {
    agent any

    tools {
        maven 'mymaven'
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
                    kubeconfig(credentialsId: 'kubeconfig-file') {
                        sh 'sed -i "s|image: .*|image: raedaffes/country-service-my-country-service2:$BUILD_NUMBER|" deployment.yaml'
                        sh 'kubectl apply -f deployment.yaml'
                        sh 'kubectl apply -f service.yaml'
                        sh 'kubectl rollout status deployment/my-country-service'
                    }
                }
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }
    }
}
