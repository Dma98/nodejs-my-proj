pipeline {

    agent {
        label 'static-slaves'
    }

    tools {
        nodejs 'nodejs'
        dockerTool 'docker-default'
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Clean') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Dma98/nodejs-my-proj'
            }
        }
        stage('Build') {
            steps {
                sh 'npm ci' //This is for building the nodejs project
            }
        }
        stage('Test') {
            steps {
                sh 'npm test' //This is for testing the nodejs modules
            }
        }
        stage('Docker build') {
            steps {
                sh 'docker build -t dmanov/nodejs-app -f Dockerfile .'
                sh 'docker tag dmanov/nodejs-app dmanov/nodejs-app${BUILD_NUMBER}'
            }
        }
        stage('Deploy') {
           steps {
                sh 'pkill node | true'
                sh 'npm install -g forever'
                sh 'forever start src/index.js'
           }
        }
    }
}