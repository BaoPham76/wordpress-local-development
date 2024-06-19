pipeline {
    agent any

    environment {
        CONTAINER_NAME = "myapp"
        DATABASE_NAME = "wordpress"
        DATABASE_PASSWORD = "password"
        DATABASE_ROOT_PASSWORD = "root_password"
        DATABASE_USER = "user"
    }

    stages {
        stage('Checkout') {
            steps {
                // Lấy mã nguồn từ repository
                git credentialsId: '938e6ad1-045b-4eee-a581-a5c12e67a672', url: 'https://github.com/BaoPham76/wordpress-local-development.git', branch: 'main'
            }
        }

        stage('Build and Deploy') {
            steps {
                script {
                    // Remove any existing containers with the same name
                    bat 'docker rm -f myapp-nginx || true'
                    bat 'docker rm -f myapp-database || true'
                    bat 'docker rm -f myapp-phpmyadmin || true'
                    bat 'docker rm -f myapp-wordpress || true'

                    // Dừng và loại bỏ các container cũ
                    bat 'docker-compose down --remove-orphans'
                    // Xây dựng và triển khai bằng Docker Compose
                    bat 'docker-compose up --build -d'
                }
            }
        }

        stage('Post-deploy Cleanup') {
            steps {
                // Dọn dẹp hệ thống Docker
                bat 'docker system prune -f'
            }
        }
    }

    post {
        always {
            // Luôn luôn lưu trữ log
            archiveArtifacts artifacts: '**/logs/**', allowEmptyArchive: true
        }
    }
}
