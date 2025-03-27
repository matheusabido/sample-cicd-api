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
                docker.build("cicd-api:latest")
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sshPublisher(publishers:
                    [sshPublisherDesc(configName: 'local-ssh-server',
                        transfers: [sshTransfer(
                            execCommand: 'docker pull cicd-api:latest && docker run cicd-api:latest -d',
                            execTimeout: 3600,
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