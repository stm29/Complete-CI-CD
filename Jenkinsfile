pipeline {
    agent any
    stages {
        stage('mkdir'){
            steps{
            sh'mkdir ios'
            sh 'mkdir /prod'
            }
        }
        stage('DeployToStaging') {
                            when {
                                    branch 'main'
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
        stage('DeployToProd') {
                            when {
                                    branch 'main'
                                 }
            steps {
                 input 'Does the staging environment look OK?'
                 milestone(1)
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
                                        remoteDirectory: '/prod')
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
