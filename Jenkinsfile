
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    sshpass -p "123" scp /var/lib/jenkins/index.html 13.234.7.156:/var/www/html
                '''
            }
        }
    }
}
