pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/opt/homebrew/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh 'export JENKINS_NODE_COOKIE=dontKillMe'
                sh 'export JENKINS_SERVER_COOKIE=dontKillMe'
                sh 'nohup /usr/local/bin/npx http-server src -p 8081 > jenkins-server.log 2>&1 < /dev/null &'
                sh 'sleep 3'
                sh 'curl -f http://localhost:8081'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully - app deployed at http://localhost:8081'
        }

        failure {
            echo 'Pipeline failed - deployment skipped. Check test results above.'
        }
    }
}