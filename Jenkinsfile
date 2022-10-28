pipeline{
    agent any
    options {
        skipStagesAfterUnstable()
    }
        environment {
        ECR_REPO_URI = "347222812711.dkr.ecr.ap-northeast-2.amazonaws.com/test_cicd"
        AWS_CREDENTIALS="TEST_CICD_JENKINS"
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
        stage('Deploy') {
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
    }
}