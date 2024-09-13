pipeline {
    agent any
    parameters {
        choice(name: 'ENVIRONMENT', choices: ['dev1', 'dev2', 'test1', 'test2'], description: 'Choose the environment')
    }
    environment{
     envName = "${params.ENVIRONMENT}"
     
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
                    if (env.envName.startsWith('dev')) {
                        topologyFile = 'dev_topology.yaml'
                    } else if (env.envName.startsWith('test')) {
                        topologyFile = 'topology.yaml'
                    }
                    // Print the selected file to the console
                    echo "Using ${topologyFile} for environment ${envName}"
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
