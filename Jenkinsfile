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
                script {
                    sh """
                      echo "Checking workspace contents:"
                      ls -l
                      echo "Checking ansible_role_health_check directory contents:"
                      ls -l ansible_role_health_check
                    """
                    stage('Health Check') {
                        dir("${env.WORKSPACE}/ansible_role_health_check") {
                            ansiblePlaybook(
                                installation: "ansible",
                                playbook: "playbook.yaml",
                                inventory: "hosts/inven",
                                extras: "-vvv"
                            )
                        }
                    }
                }
            }
            post {
                failure {
                    // Trigger the second job if the ansible script stage fails
                    build job: "restart", wait: false
                }
            }
        }
    }
}
