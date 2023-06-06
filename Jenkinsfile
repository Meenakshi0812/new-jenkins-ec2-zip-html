pipeline {
    agent any
    
    stages {
        stage('SSH to Remote Server') {
            steps {
                sshagent(credentials: ['ssh-application-server']) {
                    sh 'ssh-keyscan -H 54.87.141.198 >> ~/.ssh/known_hosts'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@54.87.141.198'
                }
            }
        }
        
        stage('Clone Git Repo as Zip') {
            steps {
                script {
                    def dateTime = new Date().format('yyyyMMdd_HHmmss')
                    sh "git clone https://github.com/example/repo.git"
                    sh "zip -r ${dateTime}.zip repo"
                }
            }
        }
        
        stage('Copy Code to /var/www/html') {
            steps {
                sshagent(credentials: ['ssh-application-server']) {
                    sh 'scp ${dateTime}.zip user@54.87.141.198:/var/www/html'
                }
            }
        }
        
        stage('Unzip Code') {
            steps {
                sshagent(credentials: ['ssh-application-server']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@54.87.141.198 'cd /var/www/html && unzip ${dateTime}.zip'"
                }
            }
        }
        
        stage('Create Softlink') {
            steps {
                sshagent(credentials: ['ssh-application-server']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@54.87.141.198 'ln -s /var/www/html/repo /var/www/html/latest'"
                }
            }
        }
    }
}
