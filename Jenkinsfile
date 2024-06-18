pipeline {
    agent any

    environment {
        CONTAINER_NAME = "my_wordpress"
        DATABASE_NAME = "wordpress"
        DATABASE_PASSWORD = "password"
        DATABASE_ROOT_PASSWORD = "root_password"
        DATABASE_USER = "user"
    }

    stages {
        stage('Checkout') {
            steps {
                // Lấy mã nguồn từ repository
                git credentialsId: 'ebbb2b83-c468-470a-9f52-e69950869d7a', url: 'https://github.com/BaoPham76/BShop.git'
            }
        }

        stage('Build and Deploy') {
            steps {
                script {
                    // Xây dựng và triển khai bằng Docker Compose
                    bat 'docker-compose down'
                    bat 'docker-compose pull'
                    bat 'docker-compose up -d --build'
                }
            }
        }

        stage('Post-deploy Cleanup') {
            steps {
                // Bất kỳ bước dọn dẹp nào sau triển khai, nếu cần
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
