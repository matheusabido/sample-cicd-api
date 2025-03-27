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
                    docker.build("cicd-api:latest", "--no-cache .")
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sshPublisher(publishers:
                    [sshPublisherDesc(configName: 'local-ssh-server',
                        transfers: [sshTransfer(
                            execCommand: 'docker stop current-cicd-api || true && docker rm current-cicd-api || true && docker run -p 3000:3000 -d --name current-cicd-api cicd-api:latest',
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