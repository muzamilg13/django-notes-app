pipeline {
    agent any

    stages {
        stage('code') {
            steps {
                git url: 'https://github.com/muzamilg13/django-notes-app.git', branch: 'main'
            }
        }

        stage('build') {
            steps {
                echo 'Running build stage'
                sh 'docker build -t my-note-app .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing to Docker Hub'
                withCredentials([usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'DOCKER_HUB_USER', passwordVariable: 'DOCKER_HUB_PASS')]) {
                    sh """
                    echo "${DOCKER_HUB_PASS}" | docker login -u "${DOCKER_HUB_USER}" --password-stdin
                    docker tag my-note-app:latest "${DOCKER_HUB_USER}/my-note-app:latest"
                    docker push "${DOCKER_HUB_USER}/my-note-app:latest"
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application'
                // Add your deployment steps here
                sh 'docker compose down && docker compose up -d'
            }
        }
    }
}
