pipeline{
    agent any

    parameters {
        string(
            name: 'APP_PORT',
            defaultValue: '3000',
            description: 'server port'
        )
    }

    environment {
        IMAGE_NAME = "jenkins-demo-app"
    }

    stages {
        stage('Checkout'){
            steps {
                echo 'checking out source code from git repo'
                checkout scm
            }
        }

        stage('check docker'){
            steps {
               bat 'docker --version'
            }
        }

        stage('dependencies'){
            steps {
             
                bat 'npm install'
            }
        }

        stage('Test APP'){
            steps {
                bat 'npm test'
            }
        }

        stage('Build'){
            steps {
                bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
            }
        }

        stage('Run Container'){
            steps {
                bat '''
                'docker run -d --name node-app-%BUILD_NUMBER% -p %APP_PORT%:3000 %IMAGE_NAME%:%BUILD_NUMBER%'
                '''  
            }
    }

        stage('verify'){
            steps {
                bat '''
                echo APP deployed successfully
                echo open http://localhost:%APP_PORT% 
                docker ps
                '''
            }
        }
}}