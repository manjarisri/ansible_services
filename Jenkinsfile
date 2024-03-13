pipeline {
    agent any
    stages {
        stage('gitcheckout') {
            steps {                
                git branch: 'main', url: 'https://github.com/manjarisri/ansible_services.git'  
            }  
        }
	    
        stage('ansible script') {
            steps {
                sh 'ansible-playbook -i hosts/inven playbook.yaml'	  
            }  
        }
        stage('Check Service Status') {
            steps {
                script {
                    def mysql_status = sh script: 'ps aux | grep mysql | grep -v grep', returnStatus: true

                    def message = ''

                    if (mysql_status == 0) {
                        message = 'MySQL service is running successfully.'
                    } else {
                        message = 'MySQL service is not running.'
                    }

                    // Send message to Cisco Spark space
                    sparkSend(
                        credentialsId: 'spark', 
                        message: message, 
                        messageType: 'text', 
                        spaceList: [[
                            spaceId: 'Y2lzY29zcGFyazovL3VybjpURUFNOnVzLXdlc3QtMl9yL1JPT00vZTAzMDVkYjAtZTA0Ny0xMWVlLWJhNmYtMjEzZTJjZjgyZTIx', 
                            spaceName: 'jenkins'
                        ]]
                    )
                }
            }
        }
    }
}
