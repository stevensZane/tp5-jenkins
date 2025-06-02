pipeline {
    agent { label 'windows' } // Utilise l'agent que tu as nommé "windows-agent"

    environment {
        DOCKER_USERNAME = "steveneszane"
        IMAGE_VERSION = "1.${BUILD_NUMBER}"
        DOCKER_IMAGE = "${DOCKER_USERNAME}/tp-app:${IMAGE_VERSION}"
        DOCKER_CONTAINER = "ci-cd-html-css-app"
    }

    stages {
        stage("Checkout") {
            steps {
                git branch: 'master', url: 'https://github.com/stevensZane/tp5-jenkins.git'
            }
        }

        stage("Test") {
            steps {
                echo "Tests en cours..."
                bat "echo Aucun test défini pour l'instant"
            }
        }

        stage("Build Docker Image") {
            steps {
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }

        stage("Push Docker Image") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'stevenszane-id', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
                    bat """
                    docker login -u %DOCKER_USER% -p %DOCKER_PASSWORD%
                    echo Docker login OK
                    docker push %DOCKER_IMAGE%
                    """
                }
            }
        }

        stage("Deploy") {
            steps {
                bat """
                docker container stop %DOCKER_CONTAINER% || exit 0
                docker container rm %DOCKER_CONTAINER% || exit 0
                docker run -d --name %DOCKER_CONTAINER% -p 8081:80 %DOCKER_IMAGE%
                """
            }
        }
    }
}
