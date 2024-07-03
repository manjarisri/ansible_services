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
            post {
                failure {
                    // Trigger the 'restart' job if the Ansible playbook stage fails
                    // build job: "test", wait: false
                    build job: 'test', wait: false, parameters: [
                            booleanParam(name: 'MANUAL_TRIGGER', value: false)
                        ]
                }
            }
        }
    }
}
