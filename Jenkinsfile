pipeline {
  agent {
    kubernetes {
      label 'my-kubernetes-agent'
    }
  }

  stages {
    stage('Clone repository') {
      steps {
        git 'https://github.com/Y3ko/spinnaker-web-jenkins'
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        script {
          // Kubeconfig dosyası Jenkins üzerindeki credentials'da tanımlanmış olmalı
          kubernetesDeploy(
            configs: 'spinnaker-web-jenkins/nginx-deployment.yaml,spinnaker-web-jenkins/nginx-ingress.yaml',
            kubeconfigId: 'kubernetes'
          )
        }
      }
    }
  }
}
