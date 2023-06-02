pipeline {
    agent any

    stages {
        stage('Pull and Zip Code') {
            steps {
                sh "rm -rf /home/ubuntu/html-zip"
                sh "cd /home/ubuntu && git clone https://github.com/Meenakshi0812/new-jenkins-ec2-zip-html.git"
                sh "cd /home/ubuntu/html-zip && zip -r code_`date +%Y%m%d%H%M%S`.zip ./*"
            }
        }

        stage('Copy Zip and Unzip') {
            steps {
                sh 'cp /home/ubuntu/html-zip/code*.zip /var/www/html/'
                sh 'cd /var/www/html && unzip -o "*.zip"'
            }
        }

        stage('Create Soft Link') {
            steps {
                sh 'ln -sf /var/www/html/index.html /var/www/html/application'
            }
        }
    }

    post {
        always {
            archiveArtifacts 'code*.zip'
            sh 'cp /var/www/html/application /var/www/html/index.html'
        }
    }
}
