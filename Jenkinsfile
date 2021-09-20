
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
    }
            stage('DeployToProduction') {
                            when {
                                    branch 'main'
                                 }
            steps{
             input 'Does the staging environment look OK?'
             milestone(1)
             sh'mkdir -p /Prod'
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
                                        remoteDirectory: '/Prod')
                                ]
                            )
                        ]
                    )
                }
            }
        post { 
        always { 
            cleanWs()
        }
    }
}
