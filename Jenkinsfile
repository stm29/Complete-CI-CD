
pipeline {
    agent any
    stages {
        stage("One"){
            echo 'hello'
        }
        stage('DeployToStaging') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
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
                                        remoteDirectory: '/var/',
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
