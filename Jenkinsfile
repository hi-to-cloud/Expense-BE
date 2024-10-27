pipeline {
    agent {
        label 'hi-to-cloud'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    stages {
        stage('pwd') {
            steps {
                sh """ 
                pwd 
                ls -l
                """
            }
        }
        stage('Build') {
            steps {
                sh """ 
                npm install
                """
            }
        }
    }
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success { 
            echo 'I will run when pipeline is success'
        }
        failure { 
            echo 'I will run when pipeline is failure'
        }
    }
}