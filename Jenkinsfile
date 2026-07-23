pipeline {

    agent any

    parameters {
        choice(
            name: 'ACTION',
            choices: ['DEPLOY', 'REMOVE'],
            description: 'Choose whether to deploy or remove application'
        )
    }

    tools {
        maven 'maven'
    }

    environment {
        APP_NAME = "book-my-ticket"
        DOCKER_IMAGE = "ranjitavaddebail/book-my-ticket"
    }

    stages {

        stage('Checkout Code') {

            when {
                expression { params.ACTION == 'DEPLOY' }
            }

            steps {
                echo "Pulling latest code from GitHub..."
                checkout scm
            }
        }

        stage('Build JAR') {

            when {
                expression { params.ACTION == 'DEPLOY' }
            }

            steps {
                echo "Building Book My Ticket Application..."
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {

            when {
                expression { params.ACTION == 'DEPLOY' }
            }

            steps {
                echo "Building Docker Image..."
                sh '''
                docker build -t $DOCKER_IMAGE:latest .
                '''
            }
        }

        stage('Docker Login & Push') {

            when {
                expression { params.ACTION == 'DEPLOY' }
            }

            steps {

                echo "Pushing Docker Image to Docker Hub..."

                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    sh '''
                    echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                    docker push $DOCKER_IMAGE:latest
                    '''
                }
            }
        }

        stage('Deploy Application') {

            when {
                expression { params.ACTION == 'DEPLOY' }
            }

            steps {
                echo "Deploying Book My Ticket using Docker Compose..."

                sh '''
                docker compose down || true
                docker compose up --build -d
                '''
            }
        }

        stage('Remove Application') {

            when {
                expression { params.ACTION == 'REMOVE' }
            }

            steps {

                echo "Stopping and Removing Application..."

                sh '''
                docker compose down
                docker image prune -af
                '''
            }
        }
    }

    post {

        success {
            echo "Book My Ticket CI/CD Pipeline executed successfully."
        }

        failure {
            echo "Book My Ticket CI/CD Pipeline failed."
        }

        always {
            echo "Pipeline execution completed."
        }
    }
}