pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "your-dockerhub-user/sample-api:latest"
        KUBE_NAMESPACE = "sample-api"
        EKS_CLUSTER = "sample-api-cluster"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/sample-api.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t $DOCKER_IMAGE .
                docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                docker push $DOCKER_IMAGE
                """
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh """
                aws eks update-kubeconfig --name $EKS_CLUSTER
                kubectl apply -f k8s/deployment.yaml -n $KUBE_NAMESPACE
                kubectl apply -f k8s/service.yaml -n $KUBE_NAMESPACE
                """
            }
        }
    }
}
