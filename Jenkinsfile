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

        stage('Deploy using Ansible playbook') {
            steps {
                kubeconfig(credentialsId: 'kubeconfig-file') {
                    ansiblePlaybook(
                        installation: 'myansible',
                        playbook: 'playbookCICD.yml',
                        inventory: 'inventory.ini',
                        vaultCredentialsId: 'ansible-vault-pass',
                        extras: "-e image_tag=${env.BUILD_NUMBER}",
                        colorized: true
                    )
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
