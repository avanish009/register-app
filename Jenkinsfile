pipeline {
    agent { label 'jenkins-agent' }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
    APP_NAME = "register-app-pipeline"
    RELEASE = "1.0.0"
    DOCKERHUB_CREDENTIALS = credentials("dockerhub")
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/avanish009/register-app.git'
            }
        }
        stage("Build Application") {
            steps {
                sh "mvn clean package"
            }
        }
        stage("Test Application") {
            steps {
                sh "mvn test"
            }
        }
        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('', DOCKERHUB_CREDENTIALS) {
                        docker_image = docker.build "${IMAGE_NAME}:${IMAGE_TAG}"
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage("Debug") {
            steps {
                script {
                    println "DOCKER_PASS = credentials("dockerhub")"
                }
            }
        }
    }
}
