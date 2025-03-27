pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git(
                    url: 'https://github.com/matheusabido/sample-cicd-api',
                    branch: 'main'
                )
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.build("cicd-api:latest")
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sshPublisher(publishers:
                    [sshPublisherDesc(configName: 'local-ssh-server',
                        transfers: [sshTransfer(
                            execCommand: 'docker run cicd-api:latest -d',
                            execTimeout: 300000,
                        )],
                        verbose: true
                    )])
                }
            }
        }
    }

    post {
        always {
            cleanWs()
            deleteDir()
        }
    }
}