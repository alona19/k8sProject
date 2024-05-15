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
                    
                    // Retrieve Kubernetes credentials
                    def kubernetesCredentials = credentials('kubernetes')

                    // Convert Kubernetes credentials to YAML format
                    def kubeconfigYaml = """
                        apiVersion: v1
                        kind: Config
                        clusters:
                        - cluster:
                            certificate-authority-data: ${kubernetesCredentials.caCertificate}
                            server: ${kubernetesCredentials.serverUrl}
                          name: my-cluster
                        contexts:
                        - context:
                            cluster: my-cluster
                            user: my-user
                          name: my-context
                        current-context: my-context
                        users:
                        - name: my-user
                          user:
                            token: ${kubernetesCredentials.token}
                    """

                    // Write the Kubernetes configuration to the file
                    writeFile file: kubeconfig, text: kubeconfigYaml

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
