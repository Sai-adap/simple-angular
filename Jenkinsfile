pipeline {
    agent any

    stages {
        stage('Install the dependencies') {
            steps {
                sh "npm install"
            }
        }

        stage('Build the code') {
            steps {
                sh "npm run ng -- build"
            }
        }

        stage('Deploy') {
            steps {
                 withCredentials([sshUserPrivateKey(credentialsId: 'angular', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                     sh '''
                        mkdir -p ~/.ssh
                        ssh-keyscan -H 13.201.3.212 >> ~/.ssh/known_hosts
                        scp -i $SSH_KEY -r build/* $SSH_USER@13.201.1.43:/var/www/html
                        ssh -i $SSH_KEY $SSH_USER@13.201.1.43 'sudo systemctl restart httpd'
                     '''
                 }   
            }
        }
    }
}
