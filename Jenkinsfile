pipeline {
    agent { label 'Jenkins-Agent' }
    environment { 
            APP_NAME = 'ci_app_pipeline'
  }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'GitHub', url: 'https://github.com/dcolanderjr/GitOps-CD-TechConsulting'
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
                    git config --global user.email "dcolanderjr@gmail.com"
                    git add deployment.yml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'GitHub', gitToolName: 'Default')]) {
                    sh "git push https://github.com/dcolanderjr/GitOps-CD-TechConsulting main"
                }
            }
        }
    }
}
