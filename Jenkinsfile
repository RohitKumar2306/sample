pipeline {
    agent any

    environment {
        GIT_BRANCH = 'main'  // Change this to your actual branch
        REMOTE_DIR = '/home/rohitkumar/Deployments'
        REMOTE_SERVER = 'RemoteServer' // The SSH server name in Jenkins
    }

    triggers {
        githubPush() // Automatically trigger when a push happens
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                echo 'Deploying files to remote server...'
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: "${REMOTE_SERVER}",
                            transfers: [
                                sshTransfer(
                                    sourceFiles: '**/*', // Deploy only text files
                                    removePrefix: '',
                                    remoteDirectory: "${REMOTE_DIR}",
                                    execCommand: 'ls -l ${REMOTE_DIR}'
                                )
                            ],
                            usePromotionTimestamp: false,
                            alwaysPublishFromMaster: true,
                            verbose: true
                        )
                    ]
                )
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}