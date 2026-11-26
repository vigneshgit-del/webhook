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





#################################################################################

pipeline {
    agent any

    tools {
        maven 'MAVEN-HOME'
    }

    stages {
        stage('Git Clone') {
            steps {
                bat 'if exist simple-java-maven-app rmdir /s /q simple-java-maven-app'
                bat 'git clone https://github.com/mekalavigneshwar9/simple-java-maven-app.git'
            }
        }

        stage('Clean') {
            steps {
                bat 'mvn clean -f simple-java-maven-app'
            }
        }

        stage('Install') {
            steps {
                bat 'mvn install -f simple-java-maven-app'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test -f simple-java-maven-app'
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package -f simple-java-maven-app'
            }
        }
    }
}




FROM tomcat:9.0
COPY target/*.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080
CMD ["catalina.sh", "run"]


services:
  web:
    image: nginx:latest
    ports:
      - "8090:80"

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: demo
      POSTGRES_PASSWORD: demo
      POSTGRES_DB: demo_db

from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from Flask + Docker!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)

    FROM python:3.10-slim
WORKDIR /app
COPY app.py /app/
RUN pip install flask
CMD ["python", "app.py"]


services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - db
      - redis
    networks:
      - app-net

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: demo
      POSTGRES_PASSWORD: demo
      POSTGRES_DB: demo_db
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - app-net

  redis:
    image: redis:alpine
    networks:
      - app-net

networks:
  app-net:

volumes:
  db-data:
