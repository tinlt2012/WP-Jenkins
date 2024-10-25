pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('b9fa9fca-6c50-4fd0-9e5e-f1b7341defda') // Đảm bảo ID này đúng
    }

    stages {
        stage('Checkout') {
            steps {
                // Lấy mã từ Git repository
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Thay thế 'your_dockerhub_username' bằng tên người dùng Docker Hub của bạn
                    def dockerImage = docker.build("tinlt/wordpress:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Production') { 
            steps {
                script {
                    // Lệnh Ansible để triển khai container. Đảm bảo rằng tệp deploy_wordpress.yml có cấu hình chính xác
                    sh 'ansible-playbook -i ansible/hosts ansible/deploy_wordpress.yml -e "image_tag=${env.BUILD_ID}"'
                }
            }
        }
    }
}