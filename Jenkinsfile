pipeline {
    agent any

    environment {
        IMAGE_NAME = "roshnipriya17/devopsfinalmini"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/roshnipriyarameshkumar/devops-finalmini.git'
            }
        }

        stage('Build React App') {
            steps {
                sh 'chmod +x build.sh'
                sh './build.sh'  // Only build the React app
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t task3 .
                docker tag task3 $IMAGE_NAME
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                docker login -u roshnipriya17 -p roshni123
                docker push $IMAGE_NAME
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                minikube delete
                minikube start
                kubectl apply -f deploy.yaml --validate=false
                kubectl apply -f svc.yaml --validate=false
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment Successful! 🎉"
        }
        failure {
            echo "Deployment Failed! ❌"
        }
    }
}
