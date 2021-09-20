
pipeline {
    agent any
    stages {
        stage('mkdir'){
            steps{
            sh'mkdir ios'
            }
        }
        stage('DeployToStaging') {
                            when {
                                    branch 'master'
                                 }
            steps {
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
