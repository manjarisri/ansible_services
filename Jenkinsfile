pipeline {
    agent any
    parameters {
        choice(name: 'ENVIRONMENT', choices: ['dev1', 'dev2', 'test1', 'test2'], description: 'Choose the environment')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Ansible Health Check') {
            steps {
                script {
                    // Define topology file based on environment
                    def topologyFile = ''
                    if (params.ENVIRONMENT.startsWith('dev')) {
                        topologyFile = 'dev_topology.yaml'
                    } else if (params.ENVIRONMENT.startsWith('test')) {
                        topologyFile = 'topology.yaml'
                    }
                    // Print the selected file to the console
                    echo "Using ${topologyFile} for environment ${params.ENVIRONMENT}"
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
}
