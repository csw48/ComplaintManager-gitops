pipeline {
    agent any
    environment {
        APP_NAME = "pass-sk-complaintmanager"
        IMAGE_TAG = "1.0.0-28" // Dynamic image tag based on build number
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/csw48/ComplaintManager-gitops'
            }
        }
        stage("Update the Deployment Tags") {
            steps {
                sh """
                    echo 'Original deployment.yaml:'
                    cat deployment.yaml
                    sed -i 's|image: .*$|image: ${APP_NAME}:${IMAGE_TAG}|g' deployment.yaml
                    echo 'Updated deployment.yaml:'
                    cat deployment.yaml
                """
            }
        }
        stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "csw48"
                    git config --global user.email "juhalala048@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest to ${IMAGE_TAG}"
                """
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh 'git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/csw48/ComplaintManager-gitops.git main'
                }
            }
        }
    }
}
