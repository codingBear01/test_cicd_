// pipeline{
//     agent any
//     options {
//         skipStagesAfterUnstable()
//     }
//         environment {
//         ECR_REPO_URI = "347222812711.dkr.ecr.ap-northeast-2.amazonaws.com/test_cicd"
//         AWS_CREDENTIALS="TEST_CICD_JENKINS"
//     }
//     stages {
//           stage('Clone repository') { 
//             steps { 
//                 script{
//                 checkout scm
//                 }
//             }
//         }
//         stage('Build') { 
//             steps { 
//                 script{
//                   app = docker.build("${ECR_REPO_URI}")
//                 }
//             }
//         }
//         stage('Test'){
//             steps {
//                   echo 'Empty'
//             }
//         }
//         stage('Deploy') {
//             steps {
//                 script{
//                     sh 'rm  ~/.dockercfg || true'
//                     sh 'rm ~/.docker/config.json || true'
//                     docker.withRegistry("https://${ECR_REPO_URI}", "ecr:ap-northeast-2:${AWS_CREDENTIALS}") {
//                     app.push("${env.BUILD_NUMBER}")
//                     app.push("latest")
//                     }
//                 }
//             }
//         }
//     }
// }

pipeline {
    agent any
    environment {
        ECR_REPO = "347222812711.dkr.ecr.ap-northeast-2.amazonaws.com/test_cicd"
        AWS_CREDENTIALS="TEST_CICD_JENKINS"
        GIT_CREDENTIAL_ID = "fe_test_account"
        NAME = "test_cicd"
        VERSION = "${env.BUILD_NUMBER}"
        GIT_URL="https://github.com/codingBear01/test_cicd_"
    }
    stages {
        stage('Git Clone') {
            steps {
                script {
                    try {
                        git url: "${GIT_URL}", branch: "main", credentialsId: "$GIT_CREDENTIALS_ID"
                        sh "sudo rm -rf ./.git"
                        env.cloneResult=true
                    } catch (error) {
                        print(error)
                        env.cloneResult=false
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
        stage('ECR Upload') {
            steps{
                script{
                    try {                       
                        withAWS(
                          credentials: "${AWS_CREDENTIALS}",
                          role: "arn:aws:iam::347222812711:user/test_cicd_jenkins", 
                          roleAccount: "347222812711", externalId:"externalId") {
                            sh "aws ecr get-login-password --region ap-northeast-2 | docker login --username AWS --password-stdin ${ECR_REPO}"
                            sh "docker build -t ${ECR_REPO} ."
                            sh "docker tag ${ECR_REPO}:${env.BUILD_NUMBER}"
                            sh "docker push ${ECR_REPO}"
                        }
                    } catch (error) {
                        print(error)
                        echo 'Remove Deploy Files'
                        sh "sudo rm -rf /var/lib/jenkins/workspace/${env.JOB_NAME}/*"
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
            post {
                success {
                    echo "The ECR Upload stage successfully."
                }
                failure {
                    echo "The ECR Upload stage failed."
                }
            }
        }
        // stage('Deploy'){
        //     steps {
        //         script{
        //             try {
        //                 withAWS(role: '{IAM Role 이름}', roleAccount: '{AWS 계정 번호}', externalId:'externalId') {
        //                     sh"""
        //                         aws ecs update-service --region ap-northeast-2 --cluster {ECS 클러스터 이름} --service {ECS 서비스 이름} --force-new-deployment
        //                     """
        //                 }
                        
        //             } catch (error) {
        //                 print(error)
        //                 echo 'Remove Deploy Files'
        //                 sh "sudo rm -rf /var/lib/jenkins/workspace/${env.JOB_NAME}/*"
        //                 currentBuild.result = 'FAILURE'
        //             }
        //         }
        //     }
        //     post {
        //         success {
        //             echo "The deploy stage successfully."
        //         }
        //         failure {
        //             echo "The deploy stage failed."
        //         }
        //     }
        // }
    }
}
