pipeline {
    agent any

    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev1', 'dev2', 'test1', 'test2'],
            description: 'Choose the environment'
        )
    }

    environment {
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
                    def topologyFile = ''

                    if (env.envName.startsWith('dev')) {
                        topologyFile = 'dev_topology.yaml'
                    } else if (env.envName.startsWith('test')) {
                        topologyFile = 'topology.yaml'
                    }

                    echo "Using ${topologyFile} for environment ${env.envName}"

                    // If your ansible command fails, post { failure } will trigger
                    // sh "ansible-playbook ${topologyFile}"
                }
            }

            post {
                failure {
                    echo "Health check failed, triggering restart job"
                    build job: 'test',
                          wait: false,
                          parameters: [
                              booleanParam(name: 'MANUAL_TRIGGER', value: false)
                          ]
                }
            }
        }
    }
}
