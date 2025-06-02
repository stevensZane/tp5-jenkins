// // Déclaration du pipeline Jenkins
// pipeline {
//     // Exécute le pipeline sur n'importe quel agent
//     agent any
//     // Déclarer les variables d'environnement globales
//     environment {
//         DOCKER_USERNAME = "steveneszane" // username docker
//         IMAGE_VERSION = "1.${BUILD_NUMBER}"  // version dynamique de l’image
//         DOCKER_IMAGE = "${DOCKER_USERNAME}/tp-app:${IMAGE_VERSION}" // nom de l’image docker
//         DOCKER_CONTAINER = "ci-cd-html-css-app"  // nom du conteneur
//     }
//     // Les étapes du pipeline
//     stages {
//         // Étape 1 : Récupération du code source depuis GitHub
//         stage("Checkout") {
//             steps {
//                 git branch: 'master', url: 'https://github.com/stevensZane/tp5-jenkins.git'
//             }
//         }
//         // Étape 2 : Exécution des tests
//         stage("Test") {
//             steps {
//                 echo "Tests en cours"
//             }
//         }
//         // Étape 3 : Création de l'image Docker
//         stage("Build Docker Image") {
//             steps {
//                 script {
//                     bat "docker build -t $DOCKER_IMAGE ."
//                 }
//             }
//         }
//         // Étape 4 : Publication de l'image sur Docker Hub
//         stage("Pubat image to Docker Hub") {
//             steps {
//                 script {
//                     withCredentials([usernamePassword(credentialsId: 'stevenszane-id', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]){
//                     bat """
//                     docker login -u ${DOCKER_USER} -p ${DOCKER_PASSWORD}
//                     echo 'Docker login successful'
//                     docker pubat $DOCKER_IMAGE
//                     """
//                     }
//                 }
//             }
//         }
//         // Étape 5 : Déploiement de l'application
//         stage("Deploy") {
//             steps {
//                 script {
//                     bat """
//                     # Arrête le conteneur s'il existe
//                     docker container stop $DOCKER_CONTAINER || true
//                     # supprime le conteneur s'il existe
//                     docker container rm $DOCKER_CONTAINER || true
//                     # Lance un nouveau conteneur en mode détaché(en arrière-plan )
//                     docker container run -d --name $DOCKER_CONTAINER -p 8081:80 $DOCKER_IMAGE
//                     """
//                 }
//             }
//         }
//     }
// }
// pipeline {
//     agent { label 'windows' } // Utilise l'agent que tu as nommé "windows-agent"

//     environment {
//         DOCKER_USERNAME = "steveneszane"
//         IMAGE_VERSION = "1.${BUILD_NUMBER}"
//         DOCKER_IMAGE = "${DOCKER_USERNAME}/tp-app:${IMAGE_VERSION}"
//         DOCKER_CONTAINER = "ci-cd-html-css-app"
//     }

//     stages {
//         stage("Checkout") {
//             steps {
//                 git branch: 'master', url: 'https://github.com/stevensZane/tp5-jenkins.git'
//             }
//         }

//         stage("Test") {
//             steps {
//                 echo "Tests en cours..."
//                 bat "echo Aucun test défini pour l'instant"
//             }
//         }

//         stage("Build Docker Image") {
//             steps {
//                 bat "docker build -t %DOCKER_IMAGE% ."
//             }
//         }

//         stage("Push Docker Image") {
//             steps {
//                 withCredentials([usernamePassword(credentialsId: 'stevenszane-id', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
//                     bat """
//                     docker login -u %DOCKER_USER% -p %DOCKER_PASSWORD%
//                     echo Docker login OK
//                     docker push %DOCKER_IMAGE%
//                     """
//                 }
//             }
//         }

//         stage("Deploy") {
//             steps {
//                 bat """
//                 docker container stop %DOCKER_CONTAINER% || exit 0
//                 docker container rm %DOCKER_CONTAINER% || exit 0
//                 docker run -d --name %DOCKER_CONTAINER% -p 8081:80 %DOCKER_IMAGE%
//                 """
//             }
//         }
//     }
// }

pipeline {
    agent { label 'windows' } // Uses your Windows agent

    environment {
        DOCKER_USERNAME = "steveneszane"
        IMAGE_VERSION = "1.${BUILD_NUMBER}"
        DOCKER_IMAGE = "${DOCKER_USERNAME}/tp-app:${IMAGE_VERSION}"
        DOCKER_CONTAINER = "ci-cd-html-css-app"
    }

    stages {
        stage("Checkout") {
            steps {
                checkout([$class: 'GitSCM', 
                         branches: [[name: 'master']], 
                         userRemoteConfigs: [[url: 'https://github.com/stevensZane/tp5-jenkins.git']]])
            }
        }

        stage("Test") {
            steps {
                echo "Tests en cours..."
                powershell "Write-Output 'Aucun test défini pour l'instant'"
            }
        }

        stage("Build Docker Image") {
            steps {
                powershell "docker build -t ${env:DOCKER_IMAGE} ."
            }
        }

        stage("Push Docker Image") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'stevenszane-id', 
                                              usernameVariable: 'DOCKER_USER', 
                                              passwordVariable: 'DOCKER_PASSWORD')]) {
                    powershell """
                    docker login -u ${env:DOCKER_USER} -p ${env:DOCKER_PASSWORD}
                    Write-Output 'Docker login OK'
                    docker push ${env:DOCKER_IMAGE}
                    """
                }
            }
        }

        stage("Deploy") {
            steps {
                powershell """
                    docker container stop ${env:DOCKER_CONTAINER} 2>&1 | Out-Null
                    docker container rm ${env:DOCKER_CONTAINER} 2>&1 | Out-Null
                    docker run -d --name ${env:DOCKER_CONTAINER} -p 8081:80 ${env:DOCKER_IMAGE}
                """
            }
        }
    }
}