pipeline {
    agent any

    tools {
        nodejs 'node16' // Ensure 'node16' is configured in Jenkins under Manage Jenkins > Global Tool Configuration
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/roohmeiy/Prime-Video-Clone.git'
            }
        }

        stage ("Trivy File Scan") {
            steps {
                sh "trivy fs . > trivy.txt"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    try {
                        // Build the Docker image tagged as 'prime-clone:latest'
                        sh "docker build -t prime-clone:latest ."
                    } catch (Exception e) {
                        error "Docker build failed: ${e.getMessage()}"
                    }
                }
            }
        }

        stage ("Trivy Image Scan") {
            steps {
                sh "trivy image prime-clone:latest"
            }
        }

        stage('Tag & Push to DockerHub') {
            steps {
                script {
                    withDockerRegistry([ credentialsId: 'Docker_hub', url: '' ]) {
                        sh "docker tag prime-clone:latest roohmeiy/prime-clone:latest"
                        sh "docker push roohmeiy/prime-clone:latest"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'k8s-cred', variable: 'KUBECONFIG_FILE')]) {
                        sh """
                            # Set the KUBECONFIG environment variable
                            export KUBECONFIG=${KUBECONFIG_FILE}
                            
                            # Navigate to the Kubernetes manifests directory
                            cd Kubernetes
                            
                            # Apply all Kubernetes manifests
                            kubectl apply -f .
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
            // Optional: Add notifications (e.g., email, Slack) here
        }
        failure {
            echo '❌ Deployment failed.'
            // Optional: Add notifications (e.g., email, Slack) here
        }
    }
}
