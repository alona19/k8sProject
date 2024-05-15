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
                        // Inside this block, you can perform any Kubernetes-related operations
                        // For example, you can apply Kubernetes manifests
                        sh "kubectl apply -f deployment.yaml -n ${NAMESPACE}"
                        sh "kubectl apply -f service.yaml -n ${NAMESPACE}"
                    }
                }
            }
        }
    }
}
