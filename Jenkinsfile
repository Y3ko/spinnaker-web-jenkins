pipeline {
    agent any
    
    environment {
        // Kubernetes namespace'ini belirtin
        NAMESPACE = 'default'
        // Uygulama adını belirtin
        APP_NAME = 'nginx-webapp'
        // Kubeconfig dosyasının yolunu belirtin
        KUBECONFIG_PATH = credentials('kubeconfig')
    }

    stages {
        stage('Deploy Nginx') {
            steps {
                // Kubeconfig dosyasını ekleyerek Nginx podunu Kubernetes'e dağıt
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'nginx-deployment.yaml',
                    namespace: 'default'
                )
            }
        }
        stage('Deploy Ingress') {
            steps {
                // Kubeconfig dosyasını ekleyerek Ingress kaynağını oluştur ve Kubernetes'e uygula
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'nginx-ingress.yaml',
                    namespace: 'default'
                )
            }
        }
        stage('Test') {
            steps {
                // Kubeconfig dosyasını ekleyerek Ingress adresini al
                sh "kubectl --kubeconfig=$KUBECONFIG_PATH get ingress -n $NAMESPACE"
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
