Jenkins GitHub Webhook Demo
Testing automatic Jenkins build through GitHub webhook.

pipeline {
    agent any

    tools {
        maven 'MAVEN-HOME'
    }

    stages {
        stage('git repo & clean') {
            steps {
                bat "if exist my-maven-project rmdir /s /q my-maven-project"
                bat "git clone https://github.com/jenkins-docs/simple-java-maven-app.git"
                bat "mvn clean -f my-maven-project"
            }
        }

        stage('install') {
            steps {
                bat "mvn install -f my-maven-project"
            }
        }

        stage('test') {
            steps {
                bat "mvn test -f my-maven-project"
            }
        }

        stage('package') {
            steps {
                bat "mvn package -f my-maven-project"
            }
        }
    }
}
