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
                script {
                    sh 'cat $HOME/master/.kube/config | kubectl apply -f - -n $NAMESPACE'
                }
            }
        }
        stage('Deploy Ingress') {
            steps {
                // Ingress kaynağını oluştur ve Kubernetes'e uygula
                script {
                    sh 'cat $HOME/master/.kube/config | kubectl apply -f - -n $NAMESPACE'
                }
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
            echo 'Hata ayrıntıları:'
            echo build.getPreviousBuild().getCauses()
        }
    }
}
