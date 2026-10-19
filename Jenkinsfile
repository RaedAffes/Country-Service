pipeline {
    agent any

    tools { 
        maven 'mymaven'
    }

    stages {

        stage('Checkout code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/RaedAffes/Country-Service.git'
            }
        }

        stage('Compile code') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Test code') {
            steps {
                sh 'mvn test'
            }

            post {
                always {
                    junit allowEmptyResults: true,
                          testResults: '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package & Deploy to Nexus') {
            steps {
                sh '''
                    WIN_IP=$(ip route | awk '/default/ {print $3}')
                    echo "Nexus host: $WIN_IP"
                    mvn deploy -DaltDeploymentRepository=nexus-snapshots::default::http://$WIN_IP:8081/repository/maven-snapshots/
                '''
            }
        }
    }
}
