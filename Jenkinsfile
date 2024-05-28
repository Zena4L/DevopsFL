pipeline {
    agent any

    stages {
        stage('Build With Maven') {
            steps {
                sh './mvnw wrapper:wrapper'
            }
        }

        stage('Build Docker Image') {
            when {
                anyOf {
                    branch 'dev'
                    branch 'staging'
                }
            }
            steps {
                sh 'docker build -t zena07/DevopFL .'
            }
        }

        stage('Login to Docker Hub') {
            when {
                anyOf {
                    branch 'main'
                    branch 'dev'
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                }
            }
        }

        stage('Push Docker Image to Registry') {
            when {
                anyOf {
                    branch 'main'
                    branch 'dev'
                }
            }
            steps {
                sh 'docker push zena07/DevopFL:latest'
            }
        }

        stage("CleanUp") {
            when {
                anyOf {
                    branch 'main'
                    branch 'staging'
                }
            }
            steps {
                sh 'docker rmi zena07/DevopFL:latest'
                sh "docker logout"
                cleanWs()
            }
        }
    }
}
