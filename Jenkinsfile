pipeline {
    agent {
        kubernetes {
            // Doğru kullanım: label 'my-kubernetes-agent'
            label 'my-kubernetes-agent'
        }
    }

    stages {
        stage('Clone repository') {
            steps {
                // 'checkout' adımını kullanarak git reposunu klonlayın.
                // 'branch' ve 'url' parametrelerini doğru şekilde kullanımı.
                checkout scm: [$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/Y3ko/spinnaker-web-jenkins']]]
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Dizileri birleştirme ve 'kubernetesDeploy' adımının doğru kullanımı.
                    def configs = ['nginx-deployment.yaml', 'nginx-ingress.yaml'].join(',')
                    // Kubernetes'e dağıtım yapmak için 'kubernetesDeploy' fonksiyonunun doğru kullanımı.
                    // Burada parantez dengesine dikkat etmek önemli.
                    kubernetesDeploy(configs: configs, kubeconfigId: 'kubernetes')
                }
            }
        }
    }
}
