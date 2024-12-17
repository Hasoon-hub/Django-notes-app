@Library("Shared") _

pipeline {
    agent { label 'ali' }

    stages {
        stage("Hello") {
            steps {
                script {
                    hello()
                }
            }
        }

        stage("Clone") {
            steps {
                script {
                    clone("https://github.com/Hasoon-hub/Django-notes-app.git", "main")
                }
            }
        }

        stage("Test") {
            steps {
                echo "Running tests..."
            }
        }

        stage('Build') {
            steps {
                script {
                    docker_build("notes-app", "latest", "hasoon")
                }
            }
        }

        stage("Push to DockerHub") {
            steps {
                script {
                    docker_push("notes-app", "latest", "hasoon")
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                sh '''
                    if [ "$(docker ps -aq -f name=db_cont)" ]; then
                        echo "Removing existing db_cont container..."
                        docker rm -f db_cont
                    else
                        echo "db_cont does not exist. Skipping removal."
                    fi

                    if [ "$(docker ps -aq -f name=django_cont)" ]; then
                        echo "Removing existing django_cont container..."
                        docker rm -f django_cont
                    else
                        echo "django_cont does not exist. Skipping removal."
                    fi

                    if [ "$(docker ps -aq -f name=nginx_cont)" ]; then
                        echo "Removing existing nginx_cont container..."
                        docker rm -f nginx_cont
                    else
                        echo "nginx_cont does not exist. Skipping removal."
                    fi

                    docker-compose up -d
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
