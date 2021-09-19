
pipeline {
    agent any
    stages {
        stage('DeployToStaging') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'web_123', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
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
                                        remoteDirectory: '/var/www/html/',
                                        execCommand: 'sudo /usr/bin/systemctl stop apache2'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}
