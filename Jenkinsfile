pipeline {
    agent any
    stages {
        stage('gitcheckout') {
            steps {                
                // Intentionally fail the job by executing a command that does not exist
                sh 'nonexistent-command'  
            }  
        }
	    
        stage('ansible script') {
            steps {
                sh 'ansible-playbook -i ansible_role_health_check/hosts/inven ansible_role_health_check/playbook.yaml'	  
            }
            post {
                failure {
                    // Trigger the second job if the ansible script fails
                    build job: 'test'
                }
            }
        }
        
        stage('Check Service Status') {
            steps {
                script {
                    // Intentionally fail the job by exiting with a non-zero status code
                    // This will cause the pipeline to fail
                    error('Intentional failure')
                }
            }
        }
    }
}
