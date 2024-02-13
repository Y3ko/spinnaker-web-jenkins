pipeline {
    agent {
        kubernetes {
            // Use 'label' with a single quote if there are spaces: label 'my-kubernetes-agent'
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
                    // Assume both YAML files are in the same directory as the Jenkinsfile:
                    configs: 'nginx-deployment.yaml', 'nginx-ingress.yaml',
                    kubeconfigId: 'kubernetes',
                )
            }
        }
    }
}
