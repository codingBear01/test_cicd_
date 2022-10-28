pipeline{
    agent any
    tools {
      nodejs "node 16.18.0"
      git "git"
    }
    environment {
        ECR_REPO_URI = "347222812711.dkr.ecr.ap-northeast-2.amazonaws.com/test_cicd"
        AWS_CREDENTIALS="ID_TEST_CICD_DEPLOY_USER"
        GIT_CREDENTIAL_ID = "fe_test_account"
        REPO_NAME = "test_cicd"
        GIT_URL="https://github.com/codingBear01/test_cicd_"
        
    }
    stages {
        stage('Pull') {
            steps {
                git url:"${GIT_URL}", 
                branch:"main", 
                poll:true, 
                changelog:true, 
                credentialsId: "${GIT_CREDENTIAL_ID}"
            }
        }
        stage('Build') {
            steps {
                sh "docker build -t ${NAME} ."
            }
        }
        stage('ECR Upload'){
            steps {
                script{
                    try{
                      withAWS(
                      credentials:"${AWS_CREDENTIALS}", 
                      role: 'arn:aws:iam::347222812711:user/test_cicd_deploy_user:role/jenkins-deploy-role', roleAccount: 'test_cicd_deploy_user', externalId:'externalId'
                      ){
                        sh "aws ecr get-login-password --region ap-northeast-2 | docker login --username AWS --password-stdin ${ECR_REPO_URI}"
                        sh "docker tag ${NAME}:latest ${ECR_REPO_URI}:latest"
                        sh "docker push ${ECR_REPO_URI}"
                      }
                    }catch(error){
                        print(error)
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
            post{
                success {
                    echo "The ECR Upload stage successfully."
                }
                failure{
                    echo "The ECR Upload stage failed."
                }
            }
        }
    }
}

