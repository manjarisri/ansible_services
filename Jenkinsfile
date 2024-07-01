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
                    """
                    stage('ansible script') {
                      steps{
            	    	sh 'ansible-playbook svc.yaml -i inven.ini'	  
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
}
