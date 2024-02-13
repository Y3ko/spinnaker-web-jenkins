pipeline {
  agent {
    kubernetes {
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
        kubernetesDeploy(
          configs: 'spinnaker-web-jenkins/nginx-deployment.yaml', 
          kubeconfigId: 'my-kubeconfig', 
          enableConfigSubstitution: true
        )
      }
    }
  }
}
