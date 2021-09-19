
pipeline {
    agent any
    stages {
        stage('mkdir'){
            steps{
            sh'mkdir ios'
            }
        }
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
