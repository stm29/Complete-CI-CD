
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
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'Staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                echo "$USERNAME",
                                echo "$USERPASS",
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'index.html',
                                        remoteDirectory: '/var/www/html',
                                        execCommand: 'sudo systemctl stop apache2 && sudo rm -rf /var/www/html/* && sudo /usr/bin/systemctl start apache2'
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
