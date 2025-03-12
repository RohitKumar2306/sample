pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/RohitKumar2306/sample.git'
        GIT_BRANCH = 'main'  // Change this to your default branch if needed
        REMOTE_DIR = '/home/rohitkumar/Deployments'
        REMOTE_SERVER = 'RemoteSession' // Name as configured in Publish over SSH
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning GitHub repository...'
                git branch: "${GIT_BRANCH}", url: "${GIT_REPO}"
            }
        }
        stage('Transfer Files to Remote Server') {
            steps {
                echo 'Transferring files to the remote server...'
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: "${REMOTE_SERVER}",
                            transfers: [
                                sshTransfer(
                                    sourceFiles: '**/*', // Transfers all files in the workspace
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
            echo 'Deployment failed.'
        }
    }
}