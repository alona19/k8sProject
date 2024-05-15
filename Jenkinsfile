
pipeline {
    agent any

    environment {
        NAMESPACE = 'devops-tools'
        DEPLOYMENT_NAME = 'frontend-app'
        SERVICE_NAME = 'frontend-app-service'
    }

    stages {
        stage('Deploy') {
            environment {
                // Specify the ID of the Kubernetes credentials configured in Jenkins
                KUBE_CREDENTIALS = credentials('kubernetes')
            }
            steps {
                script {
                    // Use the Kubernetes credentials to authenticate with GKE
                    withCredentials([kubeconfig(credentialsId: "${KUBE_CREDENTIALS}", disableVersionCheck: true)]) {
                        sh "kubectl apply -f deployment.yaml -n ${NAMESPACE}"
                        sh "kubectl apply -f frontendService.yaml -n ${NAMESPACE}"
                    }
                }
            }
        }
    }
}
