
pipeline {
    agent any
    stages {
        stage('mkdir'){
            steps{
            sh'mkdir ios'
            }
        }
        stage('DeployToStagingAllow') {
            steps {
                            when {
                                    branch 'master'
                                 }
                withCredentials([usernamePassword(credentialsId: 'web_123', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'Staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: '**/**',
                                        remoteDirectory: '/var/www/')
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
        post { 
        always { 
            cleanWs()
        }
    }
}
