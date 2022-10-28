pipeline{
    agent any
    options {
        skipStagesAfterUnstable()
    }
        environment {
        ECR_REPO_URI = "347222812711.dkr.ecr.ap-northeast-2.amazonaws.com/test_cicd"
        AWS_CREDENTIALS="TEST_CICD_JENKINS"
        CLUSTER_NAME="cosmost-board-cluster"
        SERVICE_NAME="nginx-service"
    }
    stages {
        stage('Clone repository') { 
            steps { 
                script{
                checkout scm
                }
            }
        }
        stage('Build') { 
            steps { 
                script{
                  app = docker.build("${ECR_REPO_URI}")
                }
            }
        }
        stage('Test'){
            steps {
                  echo 'Empty'
            }
        }
        stage('Upload to ECR') {
            steps {
                script{
                    sh 'rm  ~/.dockercfg || true'
                    sh 'rm ~/.docker/config.json || true'
                    docker.withRegistry("https://${ECR_REPO_URI}", "ecr:ap-northeast-2:${AWS_CREDENTIALS}") {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
        stage('Deploy to ECS') {
          steps {
                script {
                    docker.withRegistry("https://${ECR_REPO_URI}", "ecr:ap-northeast-2:${AWS_CREDENTIALS}") {
                      sh "aws ecs update-service --cluster ${CLUSTER_NAME} --service ${SERVICE_NAME} --force-new-deployment"
                    }
                }
            }
        }
    }
}
