pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000 -p 5000:5000'
        }
    }

    environment {
        CI = 'true'
    }

    stages {
        stage('build') {
            steps {
                sh 'npm install'
            }
        }

        stage('test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }

        stage('cd for dev') {
            when {
                branch 'development' 
            }

            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                input message : 'finished using the website?'
                sh './jenkins/scripts/kill.sh'
            }
        }

        stage('cd for prod') {
            when {
                branch 'production'
            }
            steps {
                sh './jenkins/scripts/deploy-for-production.sh'
                input message : 'are u ok go ?'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}