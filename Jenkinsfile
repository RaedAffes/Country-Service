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

        stage('Deploy with Ansible') {
            steps {
                withCredentials([string(credentialsId: 'DOCKER_HUB', variable: 'VAULT_PASS')]) {
                    withCredentials([file(credentialsId: 'kubeconfig-file', variable: 'KUBECONFIG')]) {
                        sh '''
                            umask 077
                            trap 'rm -f vault_pass.txt' EXIT
                            echo "$VAULT_PASS" > vault_pass.txt
                            ansible-playbook -i inventory.ini playbookCICD.yml \
                              --vault-password-file vault_pass.txt \
                              -e image_tag=$BUILD_NUMBER
                        '''
                    }
                }
            }
        }
    }

    post {
        always  { cleanWs() }
        success { echo 'Ansible playbook executed successfully!' }
        failure { echo 'Ansible playbook execution failed!' }
    }
}
