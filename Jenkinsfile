pipeline {
    agent any

    environment {
        NAMESPACE = 'devops-tools'
        DEPLOYMENT_NAME = 'frontend-app'
        SERVICE_NAME = 'frontend-app-service'
    }

    stages {
        stage('Deploy') {
            steps {
                script {
                    // Define the path to the Kubernetes configuration file
                    def kubeconfig = "${WORKSPACE}/kubeconfig.yaml"

                    // Write the Kubernetes configuration to the file
                    writeFile file: kubeconfig, text: credentials('kubernetes')

                    // Set KUBECONFIG environment variable to point to the configuration file
                    env.KUBECONFIG = kubeconfig

                    // Perform Kubernetes operations using kubectl
                    sh "kubectl apply -f deployment.yaml -n ${NAMESPACE}"
                    sh "kubectl apply -f service.yaml -n ${NAMESPACE}"
                }
            }
        }
    }
}
