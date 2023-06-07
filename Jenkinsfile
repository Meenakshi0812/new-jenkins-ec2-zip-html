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
                        checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/Meenakshi0812/new-jenkins-ec2-zip-html.git']]])

                        sh "ssh ubuntu@54.82.44.231 'cd /home/ubuntu && zip -r ${zipFileName} ${folderName}/*'"
                        sh "scp -o StrictHostKeyChecking=no /home/ubuntu/${zipFileName} ubuntu@54.82.44.231:~/var/www/html/"
                        sh "ssh ubuntu@54.82.44.231 'unzip -o ~/var/www/html/${zipFileName} -d /var/www/html'"
                        sh "ssh ubuntu@54.82.44.231 'ln -sfn /var/www/html/${folderName} /path/to/softlink'"
                    }
                }
            }
        }
    }
}
