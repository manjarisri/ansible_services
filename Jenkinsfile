pipeline {
    agent any
    parameters {
        booleanParam(name: 'MANUAL_TRIGGER', defaultValue: false, description: 'Is this a manual trigger?')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Ansible Health Check') {
            steps {
                    def manualTrigger = params.MANUAL_TRIGGER ? 'true' : 'false'
                    sh """
                        ansible-playbook -i inven.ini svc.yaml -e "manual_trigger=${manualTrigger}"
                    """
                }
        }
    }
}
