pipeline {
    agent any
    environment {
        DOCKER_CREDENTIAL_ID = 'NEXUS_CRED' // Thay bằng ID credentials trong Jenkins để kết nối với Nexus
        NEXUS_URL = 'http://192.168.127.100:8081/' // Thay bằng URL của Nexus, ví dụ: 'http://159.223.191.140:8081'
        NEXUS_REPOSITORY = 'test' // Thay bằng tên repository Docker mà bạn đã tạo trên Nexus
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Phuc03hp/wordpress_phuc.git' // Thay bằng URL repository của bạn
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("wordpress_phuc:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image to Nexus') {
            steps {
                script {
                    docker.withRegistry("${NEXUS_URL}/repository/${NEXUS_REPOSITORY}", "${DOCKER_CREDENTIAL_ID}") {
                        dockerImage.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }
        stage('Clean Up') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'docker rmi your-image-name:${env.BUILD_NUMBER}'
                    }
                }
            }
        }
    }
}

