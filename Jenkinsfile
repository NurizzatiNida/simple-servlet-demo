pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    environment {
        DEPLOY_PATH = 'C:\Program Files\tomcat10\webapps'
        WAR_NAME = 'simple-servlet-demo.war'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build WAR') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // Use %WORKSPACE% for absolute path
                bat """
                echo Deploying WAR to Tomcat...
                if exist "%DEPLOY_PATH%\\simple-servlet-demo" rmdir /s /q "%DEPLOY_PATH%\\simple-servlet-demo"
                if exist "%DEPLOY_PATH%\\%WAR_NAME%" del /f /q "%DEPLOY_PATH%\\%WAR_NAME%"
                copy /Y "%WORKSPACE%\\\\target\\\\%WAR_NAME%" "%DEPLOY_PATH%\\\\%WAR_NAME%"
                """
            }
        }
    }

    post {
        success {
            echo 'Build and deployment completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}