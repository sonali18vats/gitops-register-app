pipeline {
    agent { label "Jenkin-Agent" }

    environment {
        APP_NAME = "register-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/sonali18vats/gitops-register-app.git'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's|${APP_NAME}.*|${APP_NAME}:${IMAGE_TAG}|g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                script {
                    sh """
                        git config --global user.name "sonali18vats"
                        git config --global user.email "sonali.vats18@gmail.com"
                        git add deployment.yaml
                        git commit -m "Updated Deployment Manifest" || echo "No changes to commit"
                    """
                    withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_TOKEN')]) {
                        sh """
                            git push https://${GIT_USERNAME}:${GIT_TOKEN}@github.com/sonali18vats/gitops-register-app.git main
                        """
                    }
                }
            }
        }
    }
}
