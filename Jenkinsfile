pipeline {
    agent any

    stages {
        stage('Pull and Zip Code') {
            steps {
                script {
                    def timestamp = new Date().format("yyyy-MM-dd-HH-mm")
                    def folderName = "code_${timestamp}"
                    sshagent(['ssh-application-server']) {
                        sh """
                            ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null user@54.87.141.198 "rm -rf /home/ubuntu/${folderName}"
                            ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null user@54.87.141.198 "cd /home/ubuntu && git clone https://github.com/Meenakshi0812/new-jenkins-ec2-zip-html.git ${folderName}"
                            ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null user@54.87.141.198 "cd /home/ubuntu/${folderName} && zip -r ${folderName}.zip ./*"
                        """
                    }
                }
            }
        }

        stage('Copy Zip and Unzip') {
            steps {
                script {
                    def timestamp = new Date().format("yyyy-MM-dd-HH-mm")
                    def folderName = "code_${timestamp}"
                    sshagent(['ssh-application-server']) {
                        sh """
                            ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null user@54.87.141.198 "sudo cp /home/ubuntu/${folderName}/${folderName}.zip /var/www/html/"
                            ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null user@54.87.141.198 "cd /var/www/html && unzip -o ${folderName}.zip"
                        """
                    }
                }
            }
        }

        stage('Create Soft Link') {
            steps {
                sshagent(['ssh-application-server']) {
                    sh 'ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null user@54.87.141.198 "ln -s /var/www/html/*.zip /var/www/html/application"'
                }
            }
        }

        stage('Expose to Internet') {
            steps {
                sshagent(['ssh-application-server']) {
                    sh 'ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null user@54.87.141.198 "sudo cp /var/www/html/index.html /var/www/html/"'
                }
            }
        }
    }
}

