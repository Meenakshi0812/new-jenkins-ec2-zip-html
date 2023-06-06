pipeline {
    agent any

    stages {
        stage('Setup SSH Agent') {
            steps {
                sshagent(credentials: ['ssh-jenkins-private-key']) {
                    sh 'echo "SSH agent successfully set up"'
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    def timestamp = new Date().format("yyyy-MM-dd-HH-mm")
                    def folderName = "code_${timestamp}"
                    def zipFileName = "${folderName}.zip"

                    sshagent(credentials: ['ssh-jenkins-private-key']) {
                        sh "git branch: 'main', url: 'https://github.com/Meenakshi0812/new-jenkins-ec2-zip-html.git', directory: \"/home/ubuntu/${folderName}\""
                        sh "ssh ubuntu@75.101.201.53 'cd /home/ubuntu && zip -r ${zipFileName} ${folderName}/*'"
                        sh "scp -o StrictHostKeyChecking=no /home/ubuntu/${zipFileName} ubuntu@75.101.201.53:~/var/www/html/"
                        sh "ssh ubuntu@75.101.201.53 'unzip -o ~/var/www/html/${zipFileName} -d /var/www/html'"
                        sh "ssh ubuntu@75.101.201.53 'ln -sfn /var/www/html/${folderName} /path/to/softlink'"
                    }
                }
            }
        }
    }
}
