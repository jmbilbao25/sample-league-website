pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Pulling latest code from GitHub...'
                // Automatically handled when using Pipeline from SCM
            }
        }

        stage('Build & Test') {
            steps {
                echo 'Validating web content...'
                // Optional: Add syntax check or build commands here (e.g., npm run build)
                sh 'ls -la'
            }
        }

        stage('Deploy to Apache') {
            steps {
                echo 'Updating Apache web directory...'
                // Syncs repo files directly into Apache's document root
                // Excludes git internal files from the web root
                sh 'rsync -rltDv --exclude=".git*" --exclude="Jenkinsfile" ./ /var/www/html/'
            }
        }

        stage('Reload Apache') {
            steps {
                echo 'Reloading Apache service...'
                // Reloads Apache to ensure configuration or cached changes take effect
                sh 'sudo systemctl reload apache2 || sudo systemctl reload httpd'
            }
        }
    }

    post {
        success {
            echo 'Website successfully updated!'
        }
        failure {
            echo 'Deployment failed. Please check logs.'
        }
    }
}