pipeline {
    agent any
    // triggers {
    //     cron('*/2 * * * *') 
    // }
    stages {
        stage('gitcheckout') {
            steps {                
                git branch: 'main', url: 'https://github.com/manjarisri/ansible_services.git'  
            }  
        }
	    
        stage('ansible script') {
            steps {
                script {
                    // Intentionally fail the ansible script stage
                    error('Intentional failure in ansible script stage')
                }
            }  
            post {
                failure {
                    // Trigger the second job if the ansible script stage fails
                    build job: 'test'
                }
            }
        }
        
        stage('Check Service Status') {
            steps {
                script {
                    // Create the cache directory if it doesn't exist
                    sh """
                     if [ ! -d 'cache' ]; then mkdir 'cache'; fi
                    """

                    
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
