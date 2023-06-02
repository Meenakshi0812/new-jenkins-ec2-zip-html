pipeline {
    agent any

    stages {
        stage('Pull and Zip Code') {
            steps {
                script {
                    def timestamp = new Date().format("yyyy-MM-dd-HH-mm")
                    def folderName = "code_${timestamp}"
                    sh "rm -rf /home/ubuntu/${folderName}"
                    sh "cd /home/ubuntu && git clone https://github.com/Meenakshi0812/new-jenkins-ec2-zip-html.git ${folderName}"
                    sh "cd /home/ubuntu/${folderName} && zip -r ${folderName}.zip ./*"
                }
            }
        }

        stage('Copy Zip and Unzip') {
            steps {
                script {
                    def timestamp = new Date().format("yyyy-MM-dd-HH-mm")
                    def folderName = "code_${timestamp}"
                    sh "sudo cp /home/ubuntu/${folderName}/${folderName}.zip /var/www/html/"
                    sh "cd /var/www/html && unzip -o ${folderName}.zip"
                }
            }
        }

        stage('Create Soft Link') {
            steps {
                sh 'ln -s /var/www/html/*.zip /var/www/html/application'
            }
        }
        
        stage('Expose to Internet') {
            steps {
                sh 'sudo cp /var/www/html/index.html /var/www/html/'
            }
        }
    }
}
