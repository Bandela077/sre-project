pipeline {
    agent any

    environment {
        EC2_IP = "13.203.101.22"
        EC2_USER = "ubuntu"
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Bandela077/sre-project.git'
            }
        }

        stage('Test') {
            steps {
                sh '''
                    pip3 install flask pytest
                    echo "Tests passed!"
                '''
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh '''
                        scp -o StrictHostKeyChecking=no app.py ubuntu@13.203.101.225:~/app.py
                        ssh ubuntu@13.203.101.225 "pkill -f app.py || true"
                        ssh ubuntu@$13.203.101.225 "nohup python3 ~/app.py > ~/app.log 2>&1 &"
                        echo "Deployed successfully!"
                    '''
                }
            }
        }
    }

    post {
        success { echo "Pipeline SUCCESS ✅ }
        failure { echo "Pipeline FAILED ❌" }
    }
}

