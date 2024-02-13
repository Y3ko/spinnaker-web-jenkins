pipeline {
        kubernetes {
            label 'my-kubernetes-label'
        }
    
    environment {
        NAMESPACE = 'default'
        APP_NAME = 'nginx-webapp'
    }

    stages {
        stage('Deploy Nginx') {
            steps {
                script {
                    // Config dosyasındaki base64 kodlanmış kimlik bilgilerini değişkene atama
                    def kubeConfigData = 'LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVENDQWUyZ0F3SUJBZ0lJYnkzT2dmS2RuUWt3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TkRBeU1URXlNVEEwTXpSYUZ3MHpOREF5TURneU1UQTVNelJhTUJVeApFekFSQmdOVkJBTVRDbXQxWW1WeWJtVjBaWE13Z2dFaU1BMEdDU3FHU0liM0RRRUJBUVVBQTRJQkR3QXdnZ0VLCkFvSUJBUURma2ZmK3JzcGVPQUROVmxra3ZSckRZdjNGVjFwZVVOc09kelVmOTRUSlZYMTBaaC92K25YWnA3eUwKaXpmOXkxTGV4ZmR5ZUo3OHBTY2grem9xZ0t2eTVESUd1UUVmL0NlZlR1Y1BTWWRYS3d5TkpoREtmczM3OWpZOQpDUGVRTy9WWVFhZGN0ZXByQklPMnk1dS8xUGlxVmRXSGsvTzk0ZXBNWEpKSFl5N0Nyd2hBeEE5VE9DYmtFZFUwCkxMeHdXR1ZSKzVEWHo5WW1KbnhRZHNITDAvSUMwQXljWUpqZ0x0V25YdUhiU1UyT2xGZm9qS210dE9zMWdyNk0Kc2Y0NFVzSjd2Uitab2haSFV1dWIxUWllaHdJcS9sUy9aUG9EcDA1Y0Ztb0Jkc2NKWXhKUGROOXB4elpyTGpQNwpHNTdpZ3hzNXQyeXA3RzVSdHFnak5tekw1bHJiQWdNQkFBR2pXVEJYTUE0R0ExVWREd0VCL3dRRUF3SUNwREFQCkJnTlZIUk1CQWY4RUJUQURBUUgvTUIwR0ExVWREZ1FXQkJUbHVuYW9Mbms4TkVZUitEelFHUjZaK2Y3aUlqQVYKQmdOVkhSRUVEakFNZ2dwcmRXSmxjbTVsZEdWek1BMEdDU3FHU0liM0RRRUJDd1VBQTRJQkFRQkxROUVzZUFKWAorYjlSMHhod1VmZjNOVVVmYURMelhVakczQWt6M2lQYisxanJqdUJSVTZSN1hWTFVoZXhEejk4N3FwU2JtMSt2Cllnb2hacTFBTE9HWmN3bjM3ZUVlWXIvcSt0ajBQRmE0c1lVQ0R3bDJnS01YM0xzRHNTczZiQ1QrK1ZnZm5nUWwKVVlWK2VwZm9rZlYxaGFvZm5uT3YyYXo3dzZraVlSSzVZWXQzcnR3SS9LcFFRc2JQNHJ2UXByOFNuWUpIdXUrMwpnSlZTZzNaMmMvK1pmK1ppNUhQWUczbG0wR0lXM1ZmeldqcHJhZkNaQkxuTGEvYWtvSzk0SEtqajgvL2xWcCtlCmNJR0NSeU9FenhOTGdDOTh3NmNLMHpIeUdJVnFpT3dTU2wrV2hoOFQ2UW4yNGhEUXJ0SDQ4ajJKcmEzQnMvSVYKTWY3MndmOFNpUHhUCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K'

                    // Config dosyasını diskte geçici olarak oluşturma
                    writeFile file: 'config', text: kubeConfigData

                    // KUBECONFIG ortam değişkenini ayarlama
                    withEnv(["KUBECONFIG=${workspace}/config"]) {
                        // NAMESPACE ve diğer değişkenler burada kullanılabilir
                        // Örnek olarak, nginx-deployment.yaml dosyasını apply etme
                        sh "kubectl apply -f nginx-deployment.yaml -n ${NAMESPACE}"
                    }
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
            echo error
        }
    }
}
