pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "your-registry/your-app:latest"
    }

    stages {
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarQubeScanner'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform init'
                sh 'terraform apply -auto-approve'
            }
        }

        stage('Ansible Playbook') {
            steps {
                ansiblePlaybook playbook: 'playbook.yml', inventory: 'inventory'
            }
        }

        stage('Docker Build & Push') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE} .'
                sh 'docker push ${DOCKER_IMAGE}'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}
