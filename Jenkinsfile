pipeline {
    agent any
    stages {
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

