pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Ansible Health Check') {
            steps {
                echo "Checking workspace contents:"
                sh "ls -l"

                // Execute Ansible playbook
                sh 'ansible-playbook svc.yaml -i inven.ini'
            }
        }
    }
}
