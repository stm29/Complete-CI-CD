pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh 'gradle build --no-daemon'
                archiveArtifacts artifacts: 'index.html'
            }
        }
        stage('DeployToStaging') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'web_123', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        configName: 'staging',
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'index.html',
                                        remoteDirectory: '/var/www/html',
                                        execCommand: 'sudo /usr/bin/systemctl stop apache2 && rm -rf /var/www/html/* && sudo /usr/bin/systemctl start apache2'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
        stage('DeployToProduction') {
            steps {
                input 'Does the staging environment look OK?'
                milestone(1)
                withCredentials([usernamePassword(credentialsId: 'web_123', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        configName: 'staging',
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'index.html',
                                        remoteDirectory: '/var/www/html',
                                        execCommand: 'sudo /usr/bin/systemctl stop apache2 && rm -rf /var/www/html/* && sudo /usr/bin/systemctl start apache2'
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
