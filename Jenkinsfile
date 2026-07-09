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
}