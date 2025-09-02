pipeline {
    agent any

    tools {
        maven 'Maven' // À configurer dans Jenkins (Manage Jenkins > Global Tool Configuration)
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/n-abdoulAziz/lab5-jenkins-tp6.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy (local Docker)') {
            steps {
                sh 'docker-compose up -d --build'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                // Utilise les credentials DockerHub configurés dans Jenkins
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub_credentials', 
                    passwordVariable: 'DOCKER_HUB_PASSWORD', 
                    usernameVariable: 'DOCKER_HUB_USERNAME')]) {

                    // Se connecter à DockerHub
                    sh "echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin"

                    // Taguer l'image locale et la pousser sur DockerHub
                    sh "docker tag lab5-jenkins-tp6:latest $DOCKER_HUB_USERNAME/lab5-jenkins-tp6:v$BUILD_NUMBER"
                    sh "docker push $DOCKER_HUB_USERNAME/lab5-jenkins-tp6:v$BUILD_NUMBER"
                }
            }
        }
    }
}
