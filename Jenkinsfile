pipeline {
    agent any
    tools {
        nodejs 'mynode'
    }
 
    stages {
        stage('git cloning') {
            steps {
                echo 'cloning files from github'
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Sakshipadgham123/Nodepipeline.git']])
            }
        }
        stage('Build') {
            steps {
                echo 'Building nodejs project'
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing project'
                sh './node_modules/mocha/bin/_mocha --exit ./test/test.js'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying nodejs project on live server'
                script{
                    sshagent(['0316f15d-5168-45f5-a789-6ee4ff26db3b']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ec2-user@52.66.80.87<<EOF
                        cd /home/ec2-user/Nodejs-app
                        git pull https://github.com/Sakshipadgham123/Nodepipeline.git
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
}
