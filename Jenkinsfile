pipeline {
    agent any

    options {
        // Delete the workspace before starting the build
        skipStagesAfterUnstable()
        disableConcurrentBuilds()
    }

    environment {
        ANSIBLE_SERVER = 'ansible-server' // SSH server for ansible deployment
    }

    triggers {
        // Poll SCM every minute (you might want to change this to a more reasonable schedule)
        pollSCM('* * * * *')
        // GitHub hook trigger for GIT SCM polling
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the Git repository
                git url: 'https://github.com/Omar-Abdelwahabb1/cafe-app.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Running an Ansible playbook to create the Docker image
                sshagent(credentials: ['ansible-ssh-credentials']) {
                    sh 'ansible-playbook /opt/docker/create-image-cafe-app.yml'
                }
            }
        }

        stage('Test') {
            steps {
                // Example of a test stage (adjust based on your project)
                echo 'Running unit tests...'
                // Add your testing commands here
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Running an Ansible playbook to deploy the app
                sshagent(credentials: ['ansible-ssh-credentials']) {
                    sh 'ansible-playbook /opt/docker/k8s-deploy.yml'
                }
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully!'
        }
        failure {
            echo 'CI/CD pipeline failed.'
        }
    }
}

