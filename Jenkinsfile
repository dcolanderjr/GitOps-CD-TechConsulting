pipeline {
    agent { label 'Jenkins-Agent' }
    environment { 
            APP_NAME = 'pipeline_project_app'
  }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/dcolanderjr/gitops-pipeline-app'
            }
        }

        stage('Update the Deployment Tags') {
            steps {
                sh """
                    cat deployment.yml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                    cat deployment.yml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                    git config --global user.name "dcolanderjr"
                    git config --global user.email "< YOUR EMAIL >"
                    git add deployment.yml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/dcolanderjr/gitops-pipeline-app main"
                }
            }
        }
    }
}
