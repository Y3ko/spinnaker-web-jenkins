pipeline {
    agent {
        kubernetes {
            label 'my-kubernetes-label'
        }
    }
    
    environment {
        NAMESPACE = 'default'
        APP_NAME = 'nginx-webapp'
    }

    stages {
        stage('Deploy Nginx') {
            steps {
                kubernetesDeploy(configs: 'nginx-deployment.yaml')
            }
        }
        stage('Deploy Ingress') {
            steps {
                kubernetesDeploy(configs: 'nginx-ingress.yaml')
            }
        }
        stage('Test') {
            steps {
                sh "kubectl get ingress -n $NAMESPACE"
            }
        }
    }

    post {
        success {
            echo 'Nginx podu ve Ingress başarıyla dağıtıldı ve test edildi!'
        }
        failure {
            echo 'Nginx podu veya Ingress dağıtılırken veya test edilirken hata oluştu.'
            echo 'Hata ayrıntıları:'
            echo error
        }
    }
}
