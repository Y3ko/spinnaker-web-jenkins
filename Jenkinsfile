pipeline {
  agent any

  environment {
    // Specify Kubernetes namespace
    NAMESPACE = 'default'
    // Specify application name
    APP_NAME = 'nginx-webapp'
  }

  stages {
    stage('Deploy Nginx') {
      steps {
        // Deploy Nginx pod to Kubernetes
        kubernetesDeploy(
          configs: 'nginx-deployment.yaml', // Adjust the path to your deployment YAML file
          kubeconfigId: 'my-kubeconfig', // Kubernetes configuration ID defined in Jenkins configuration
          namespace: "${env.NAMESPACE}" // Use proper syntax to access environment variable
        )
      }
    }
    stage('Deploy Ingress') {
      steps {
        // Create and apply Ingress resource to Kubernetes
        kubernetesDeploy(
          configs: 'nginx-ingress.yaml', // Adjust the path to your Ingress YAML file
          kubeconfigId: 'my-kubeconfig', // Kubernetes configuration ID defined in Jenkins configuration
          namespace: "${env.NAMESPACE}" // Use proper syntax to access environment variable
        )
      }
    }
    stage('Test') {
      steps {
        // Get Ingress address
        script {
          def ingress = sh(script: "kubectl get ingress -n ${env.NAMESPACE} -o jsonpath='{.items[0].status.loadBalancer.ingress[0].ip}'", returnStdout: true).trim()
          echo "Ingress address: $ingress"
        }
      }
    }
  }

  post {
    success {
      echo 'Nginx pod and Ingress successfully deployed and tested!'
    }
    failure {
      echo 'Error occurred while deploying Nginx pod or Ingress or during testing.'
      echo 'Error details:'
      echo build.getPreviousBuild().getCauses()
    }
  }
}
