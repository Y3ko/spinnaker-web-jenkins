pipeline {
    agent {
        kubernetes {
            // Jenkins ajanının Kubernetes üzerinde çalışmasını sağla
            label 'my-kubernetes-label'
        }
    }
    
    environment {
        // Kubernetes namespace'ini belirtin
        NAMESPACE = 'default'
        // Uygulama adını belirtin
        APP_NAME = 'nginx-webapp'
        // Kubeconfig dosyasının yolunu belirtin
        KUBECONFIG_PATH = "$HOME/master/.kube/config"
    }

    stages {
        stage('Deploy Nginx') {
            steps {
                // Kubeconfig dosyasını ekleyerek Nginx podunu Kubernetes'e dağıt
                script {
                    sh "kubectl --kubeconfig=$KUBECONFIG_PATH apply -f nginx-deployment.yaml -n $NAMESPACE"
                }
            }
        }
        stage('Deploy Ingress') {
            steps {
                // Kubeconfig dosyasını ekleyerek Ingress kaynağını oluştur ve Kubernetes'e uygula
                script {
                    sh "kubectl --kubeconfig=$KUBECONFIG_PATH apply -f nginx-ingress.yaml -n $NAMESPACE"
                }
            }
        }
        stage('Test') {
            steps {
                // Kubeconfig dosyasını ekleyerek Ingress adresini al
                script {
                    sh "kubectl --kubeconfig=$KUBECONFIG_PATH get ingress -n $NAMESPACE"
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
            echo 'Hata ayrıntıları:'
            echo build.getPreviousBuild().getCauses()
        }
    }
}
