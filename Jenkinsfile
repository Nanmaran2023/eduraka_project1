pipeline {
    agent {
        label 'project1'
    }
    environment {
        DOCKER_IMAGE_NAME = "devrajshourya/train-schedule"
    }

    stages {
    	stage('Checkout'){
    	    steps {
    	    	git branch: 'main', url: 'https://github.com/DevShourya/myproject1.git'
    	    	//checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/DevShourya/myproject1.git']])
    	    }
    	}
    	stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
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
//        stage('CanaryDeploy') {
//            environment { 
//                CANARY_REPLICAS = 1
//            }
//            steps {
//               kubernetesDeploy(
//                    kubeconfigId: 'my-aks-cluster',
//                    configs: 'train-schedule-kube-canary.yml',
//                   enableConfigSubstitution: true
//                )
//            }
//        }
    }
}
