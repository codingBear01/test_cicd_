// pipeline{
//     // agent any
//     // tools {
//     //   nodejs "node 16.18.0"
//     //   git "git"
//     // }
//     // environment {
//     //     ECR_REPO_URI = "347222812711.dkr.ecr.ap-northeast-2.amazonaws.com/test_cicd"
//     //     AWS_CREDENTIALS="TEST_CICD_JENKINS"
//     //     GIT_CREDENTIAL_ID = "fe_test_account"
//     //     REPO_NAME = "test_cicd"
//     //     GIT_URL="https://github.com/codingBear01/test_cicd_"
        
//     // }
//     // stages {
//     //     stage('Pull') {
//     //         steps {
//     //             git url:"${GIT_URL}", 
//     //             branch:"main", 
//     //             poll:true, 
//     //             changelog:true, 
//     //             credentialsId: "${GIT_CREDENTIAL_ID}"
//     //         }
//     //     }
//     //     stage('Build') {
//     //         steps {
//     //             sh "docker build -t ${REPO_NAME} ."
//     //         }
//     //     }
//     //     stage('ECR Upload'){
//     //         steps {
//     //             script{
//     //                 try{
//     //                   withAWS(
//     //                   credentials:"${AWS_CREDENTIALS}", 
//     //                   role: "deploy-role", roleAccount: "347222812711", externalId:"externalId"
//     //                   ){
//     //                     sh "aws ecr get-login-password --region ap-northeast-2 | docker login --username AWS --password-stdin ${ECR_REPO_URI}"
//     //                     sh "docker tag ${REPO_NAME}:latest ${ECR_REPO_URI}:latest"
//     //                     sh "docker push ${ECR_REPO_URI}"
//     //                   }
//     //                 }catch(error){
//     //                     print(error)
//     //                     currentBuild.result = 'FAILURE'
//     //                 }
//     //             }
//     //         }
//     //         post{
//     //             success {
//     //                 echo "The ECR Upload stage successfully."
//     //             }
//     //             failure{
//     //                 echo "The ECR Upload stage failed."
//     //             }
//     //         }
//     //     }
//     // }
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

node {
      environment {
        ECR_REPO_URI = "347222812711.dkr.ecr.ap-northeast-2.amazonaws.com/test_cicd"
        AWS_CREDENTIALS="TEST_CICD_JENKINS"
      }
      stage('Clone repository') {
          checkout scm
      }

      stage('Build image') {
          app = docker.build("${ECR_REPO_URI}")
      }

      stage('Push image') {
          sh 'rm  ~/.dockercfg || true'
          sh 'rm ~/.docker/config.json || true'
          
          docker.withRegistry('https://${ECR_REPO_URI}', 'ecr:ap-northeast-2:${AWS_CREDENTIALS}') {
          app.push("${env.BUILD_NUMBER}")
          app.push("latest")
     }
  }
}