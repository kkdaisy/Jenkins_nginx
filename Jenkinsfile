pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker') // Docker Hub 자격 증명
        IMAGE_NAME = "kkmee0209/nginxtest" // Docker 이미지 이름
        LATEST_TAG = "latest" // latest 태그
        BUILD_TAG = "${BUILD_NUMBER}" // 빌드 번호를 태그로 사용
        KUBE_CONFIG = credentials('kube-config') // Kubernetes 클러스터 구성 파일 (Kubeconfig) 자격 증명
    }

    stages {
        stage('Checkout') {
            steps {
                // 소스 코드 체크아웃 (예: Git, SVN 등)
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    
                    // Docker 이미지 빌드 (빌드 번호 태그)
                    def buildImage = docker.build("${IMAGE_NAME}:${BUILD_TAG}", "--file Dockerfile .")

                    // Docker Hub에 로그인
                    docker.withRegistry('https://index.docker.io/v1/', 'docker') {
                        // Docker 이미지 푸시 (latest 태그)
                        latestImage.push()
                        
                        // Docker 이미지 푸시 (빌드 번호 태그)
                        buildImage.push()
                    }
                }
            }
        }

        stage('Deploy to AKS') {
            steps {
                script {
                    // Kubernetes 클러스터에 연결
                    withKubeConfig(credentialsId: 'kube-config', doNotReplace: true) {
                        // Kubernetes 클러스터에 배포 (latest 태그)
                        sh "kubectl set image deployment/nginx-deployment nginx=${IMAGE_NAME}:${BUILD_TAG}"
                    }
                }
            }
        }
    }
}
