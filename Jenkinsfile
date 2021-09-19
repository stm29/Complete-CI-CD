
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
        stage ('Deploy') {
    steps{
        sshagent(credentials : ['use-the-id-from-credential-generated-by-jenkins']) {
            sh 'ssh -o StrictHostKeyChecking=no sam@13.234.7.156 uptime'
            sh 'ssh -v sam@13.234.7.156'
            sh 'scp ./index.html sam@13.234.7.156:/var/www/html'
        }
    }
}
    }
}
