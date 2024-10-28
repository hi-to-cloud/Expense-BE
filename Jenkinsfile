pipeline {
    agent {
        label 'hi-to-cloud'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    environment {
        def appVersion = '' //define variable
        def nexusUrl ='nexus.step-into-iot.cloud:8081'
        def repository = 'backend'
        def credentialsId ='nexus-auth'
    }
    stages {
        stage('Working Dir') {
            steps {
                sh """ 
                pwd 
                ls -l
                """
            }
        }
        stage('App Version'){
            steps{
                script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "App Version: ${appVersion}"
                }
            }
        }
        stage('Installing Dependencies') {
            steps {
                sh """ 
                echo 'App Version: ${appVersion}'
                echo "Start Installing Dependencies................"
                npm install
                echo "Installing Dependencies Done................"
                """
            }
        }
        stage('Build') {
            steps {
                sh """ 
                zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                ls -ltr
                """
            }
        }
        stage('Nexus'){
            steps {
                script{
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${nexusUrl}",
                        groupId: 'com.expense',
                        version: "${appVersion}",
                        repository: "${repository}",
                        credentialsId: "${credentialsId}",
                        artifacts: [
                            [artifactId: "${repository}",
                            classifier: '',
                            file: "${repository}-" + "${appVersion}" + '.zip',
                            type: 'zip']
                        ]
                    )
                }
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