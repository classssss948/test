pipeline {
    agent any  // 在任何可用节点运行
    stages {
        // 阶段 1：拉取代码
        stage('Checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/classss948/test.git', 
                credentialsId: 'github-token'  // 使用你配置的 GitHub Token 凭据 ID
            }
        }
        // 阶段 2：构建 Docker 镜像
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("my-web:${env.BUILD_ID}")
                }
            }
        }
        // 阶段 3：推送镜像到仓库（可选）
        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://your-registry.com', 'docker-credentials') {
                        dockerImage.push()
                    }
                }
            }
        }
        // 阶段 4：部署到 Kubernetes（如需）
        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'k8s-config']) {
                    sh 'kubectl apply -f k8s-deployment.yaml'
                }
            }
        }
    }
}
