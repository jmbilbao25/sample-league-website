pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    environment {
        SERVERS = 'ubuntu@32.198.43.226 ubuntu@34.205.2.151'
        DOCROOT = '/var/www/html'
        APP_SRC = './'
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build & Test') {
            steps {
                sh 'echo "No build step — deploying repo as-is."'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(credentials: ['webservers-ssh-key']) {
                    sh '''
                        set -eu
                        SSH_OPTS="-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null"
                        for HOST in ${SERVERS}; do
                            echo "=== Deploying to ${HOST}:${DOCROOT} ==="
                            rsync -az --delete -e "ssh ${SSH_OPTS}" --rsync-path="sudo rsync" \
                                --exclude '.git' --exclude 'Jenkinsfile' \
                                "${APP_SRC}" "${HOST}:${DOCROOT}/"
                            ssh ${SSH_OPTS} "${HOST}" "sudo systemctl reload apache2"
                            echo "=== ${HOST} updated ==="
                        done
                    '''
                }
            }
        }
    }
}
