pipeline {
    agent {
        kubernetes {
            label 'my-kubernetes-label'
        }
    }
    
    environment {
        NAMESPACE = 'default'
        APP_NAME = 'nginx-webapp'
        KUBECONFIG_PATH = credentials('$HOME/master/.kube/config')
    }

    stages {
        stage('Deploy Nginx') {
            steps {
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'nginx-deployment.yaml',
                    namespace: 'default'
                )
            }
        }
        stage('Deploy Ingress') {
            steps {
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'nginx-ingress.yaml',
                    namespace: 'default'
                )
            }
        }
        stage('Test') {
            steps {
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
