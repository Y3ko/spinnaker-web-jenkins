pipeline {
    agent {
        kubernetes {
            // Use 'label' with a single quote if there are spaces: label 'my-kubernetes-agent'
            label 'my-kubernetes-agent'
        }
    }

    stages {
        stage('Clone repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Y3ko/spinnaker-web-jenkins'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    def configs = ['nginx-deployment.yaml', 'nginx-ingress.yaml'].join(',')
                    // Yukarıdaki satır, diziyi virgülle ayrılmış bir dizeye dönüştürür
                    kubernetesDeploy(configs: configs, kubeconfigId: 'kubernetes')
                )
            }
        }
    }
}
