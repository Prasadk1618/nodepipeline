pipeline {
    agent any
    tools {
        nodejs 'mynode'
    }

    stages {
        stage('Git Clonening') {
            steps {
                echo 'Git Checkout'
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Prasadk1618/nodepipeline.git']])
            }
        }
        stage('Build') {
            steps {
                echo 'Building Nodejs-app'
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                sh './node_modules/mocha/bin/_mocha --exit ./test/test.js'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
                sshagent(['38515a03-35c6-446f-b684-c87614e1b177']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@15.207.116.22 <<EOF
                        cd /home/ubuntu/nodeapp/
                        git pull https://github.com/Prasadk1618/nodepipeline.git
                        npm install
                        sudo npm install -g pm2
                        pm2 restart index.js || pm2 start index.js
                        exit
                        EOF
                    '''
                }
            }
        }
    }
}
