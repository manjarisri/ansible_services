pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
         }  
       
	    
        stage('ansible script') {
            steps {
                script {
                    sh """
                      ls -l
                    """
                    stage('healthcheck'){
                     dir("${env.WORKSPACE}/ansible_role_health_check"){
                       ansiblePlaybook(
			 installation: "ansible",
		         playbook: "./playbook.yaml",
                         inventory: "./hosts/inven",
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
