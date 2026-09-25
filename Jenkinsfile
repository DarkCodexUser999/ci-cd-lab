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
                sh '''
                    export JENKINS_NODE_COOKIE=dontKillMe
                    nohup npx http-server src -p 8081 > jenkins-server.log 2>&1 &
                    echo $! > jenkins-server.pid
                '''
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

