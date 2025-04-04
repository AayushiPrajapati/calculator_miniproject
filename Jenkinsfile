pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = 'calculator_miniproject'
        GITHUB_REPO_URL = 'https://github.com/AayushiPrajapati/calculator_miniproject.git'
    }

    stages {
        stage('Clone Git') {
            steps {
                git branch: 'main', url: "${GITHUB_REPO_URL}"
            }
        }

        stage('Build the Maven Project') {
            steps {
                // Build the Maven project
                sh 'mvn clean package'
            }
        }
        
        stage('Test the Maven project') { 
            steps {
                sh 'mvn test' 
            }
        }
         stage('Verify JAR Existence') {
            steps {
                sh 'ls -lh target/'   // Check if the JAR file is actually created
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_NAME}", '.')
                }
            }
        }


        stage('Push the Docker Image to Docker Hub') {
            steps {
                script{
                    docker.withRegistry('', 'dockerhub_cred') {
                    sh "docker tag ${DOCKER_IMAGE_NAME} aayushi6402/calculator_miniproject:latest"
                    sh 'docker push aayushi6402/calculator_miniproject:latest'
                    }
                 }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                script {
                    ansiblePlaybook(
                        playbook: 'deploy.yml',
                        inventory: 'inventory'
                    )
                }
            }
        }
    }
}
