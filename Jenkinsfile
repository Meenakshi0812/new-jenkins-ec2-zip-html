pipeline {
    agent any

    stages {
        stage('Pull and Zip Code') {
            steps {
                sh "rm -rf /home/ubuntu/new-jenkins-ec2-zip-html"
                sh "cd /home/ubuntu && git clone https://github.com/Meenakshi0812/new-jenkins-ec2-zip-html.git"
                sh "cd /home/ubuntu/new-jenkins-ec2-zip-html && zip -r code_`date +%Y%m%d%H%M%S`.zip ./*"
            }
        }

        stage('Copy Zip and Unzip') {
            steps {
                sh 'cp /home/ubuntu/new-jenkins-ec2-zip-html/code*.zip /var/www/html/'
                sh 'cd /var/www/html && unzip -o "*.zip"'
            }
        }

        stage('Configure Apache') {
            steps {
                sh 'cp /var/www/html/index.html /var/www/html/index.html.backup'
                sh 'cp /var/www/html/index.html /var/www/html/application.html'
                sh 'systemctl start apache2'
            }
        }
    }
}
