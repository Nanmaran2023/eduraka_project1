pipeline {
    agent {
        label 'project1'
    }
    environment {
        DOCKER_IMAGE_NAME = "devrajshourya/train-schedule"
    }

    stages {
        stage('Build Docker Image') {
            when {
                branch 'main'
            }
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
            }
        }
        stage('Push Docker Image') {
           
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-creds') {
                        app.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }
        stage('CanaryDeploy') {
            
            environment { 
                CANARY_REPLICAS = 1
            }
            steps {
                kubernetesDeploy(
                    kubeconfigId: 'my-aks-cluster',
                    configs: 'train-schedule-kube-canary.yml',
                    enableConfigSubstitution: true
                )
            }
        }
    }
}
