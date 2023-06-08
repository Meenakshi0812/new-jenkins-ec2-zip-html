pipeline {
    agent any

    stages {
        stage('Clone Git Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Meenakshi0812/new-jenkins-ec2-zip-html.git'
            }
        }

        stage('deploy to ec2') {
            steps {
                sshagent(['remote-ssh']) {
                    script {
                        def timestamp = new Date().format("yyyy-MM-dd-HH-mm")
                        def folderName = "code_${timestamp}"
                        def zipFileName = "${folderName}.zip"

                        sh "scp -o StrictHostKeyChecking=no -r /home/ec2-user/* ubuntu@3.80.125.254:/var/www/html/${folderName}/"
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@3.80.125.254 'cd /var/www/html/${folderName} && zip -r /var/www/html/${zipFileName} *'"
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@3.80.125.254 'unzip -o /var/www/html/${zipFileName} -d /var/www/html'"
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@3.80.125.254 'ln -sfn /var/www/html/${folderName} /var/www/html/code'"
                    }
                }
            }
        }
    }
}
