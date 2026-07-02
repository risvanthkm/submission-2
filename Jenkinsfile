pipeline {
    agent any

    environment {
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
        USER_IMAGE = "risvanthkm/user-service:latest"
        ORDER_IMAGE = "risvanthkm/order-service:latest"
    }

    stages {
        stage('Build User Service') {
            steps {
                dir('backend/user-service') {
                    sh '/usr/local/go/bin/go build '
                }
            }
        }

        stage('Test User Service') {
            steps {
                dir('backend/user-service') {
                    sh '/usr/local/go/bin/go test ./...'
                }
            }
        }

        stage('Build Order Service') {
            steps {
                dir('backend/order-service') {
                    sh '/usr/local/go/bin/go build'
                }
            }
        }

        stage('Test Order Service') {
            steps {
                dir('backend/order-service') {
                    sh '/usr/local/go/bin/go test ./...'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                sh """
                docker build -t $USER_IMAGE backend/user-service
                docker build -t $ORDER_IMAGE backend/order-service
                """
            }
        }

        stage('Push Images') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin

                    docker push $USER_IMAGE
                    docker push $ORDER_IMAGE
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl apply -f k8s/user-service/
                kubectl apply -f k8s/order-service/

                kubectl rollout restart deployment/user-service
                kubectl rollout restart deployment/order-service
                """
            }
        }

    }
}
        
