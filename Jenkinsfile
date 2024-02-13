pipeline {
    agent any
    
    environment {
        // Kubernetes namespace'ini belirtin
        NAMESPACE = 'default'
        // Uygulama adını belirtin
        APP_NAME = 'nginx-webapp'
    }
    
    stages {
        stage('Deploy Nginx') {
            steps {
                // Nginx podunu Kubernetes'e dağıt
                kubernetesDeploy(
                    configs: 'nginx-deployment.yaml',
                    kubeconfigId: 'my-kubeconfig', // Jenkins yapılandırmasında tanımlı kubeconfig kimliği
                    namespace: NAMESPACE
                )
            }
        }
        stage('Deploy Ingress') {
            steps {
                // Ingress kaynağını oluştur ve Kubernetes'e uygula
                kubernetesDeploy(
                    configs: 'nginx-ingress.yaml',
                    kubeconfigId: 'my-kubeconfig', // Jenkins yapılandırmasında tanımlı kubeconfig kimliği
                    namespace: NAMESPACE
                )
            }
        }
        stage('Test') {
            steps {
                // Ingress adresini al
                script {
                    sh "kubectl get ingress -n $NAMESPACE"
                }
            }
        }
    }
    
    post {
        success {
            echo 'Nginx podu ve Ingress başarıyla dağıtıldı ve test edildi!'
        }
        failure {
            echo 'Nginx podu veya Ingress dağıtılırken veya test edilirken hata oluştu.'
        }
    }
}
