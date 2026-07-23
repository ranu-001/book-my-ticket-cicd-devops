<<<<<<< HEAD
pipeline {
    agent any

    parameters {
        choice(
            name: 'ACTION',
            choices: ['DEPLOY', 'REMOVE'],
            description: 'Choose whether to deploy or remove containers'
        )
    }

    tools {
        maven 'maven'
    }

    environment {
        APP_NAME = "book-my-ticket"
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                echo "Fetching latest source code from GitHub..."
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

        stage('Deploy Application') {
            when {
                expression { params.ACTION == 'DEPLOY' }
            }
            steps {
                echo "Building Docker Image and Starting Containers..."
                sh 'docker compose down || true'
                sh 'docker compose up --build -d'
            }
        }

        stage('Remove Application') {
            when {
                expression { params.ACTION == 'REMOVE' }
            }
            steps {
                echo "Stopping and Removing Containers..."
                sh 'docker compose down'
                sh 'docker image prune -af'
            }
        }
    }

    post {

        success {
            echo "Book My Ticket deployed successfully."
        }

        failure {
            echo "Pipeline execution failed."
        }

        always {
            echo "Pipeline execution completed."
        }
    }
=======
pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {
        stage('build stage') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo "build success"
                }
                failure {
                    echo "build failure"
                }
            }
        }
        stage('build test') {
            steps {
                sh 'mvn test'
            }
            post {
                success {
                    echo "test success"
                }
                failure {
                    echo "test failure"
                }
            }
        }
       
    }
    post {
        success {
            echo "pipeline success"
        }
        failure {
            echo "pipeline failure"
        }
    }
>>>>>>> a7162fe33fc7e34b28e054bc17a3324473ea0e05
}