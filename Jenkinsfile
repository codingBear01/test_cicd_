pipeline{
    agent any
    tools {
      nodejs "node 16.18.0"
      git "git"
    }
    environment {
        ECR_REPO = "347222812711.dkr.ecr.ap-northeast-2.amazonaws.com/test_cicd"
        AWS_CREDENTIALS="ID_DEPLOY_USER"
        GIT_CREDENTIAL_ID = "fe_test_account"
        NAME = "test_cicd"
        GIT_URL="https://github.com/codingBear01/test_cicd"
        ENCRYPTED_TOKEN="eyJwYXlsb2FkIjoiOFM2SWsrMVk5SzAzV2lzQTNSdFhoQWVxUWJIb1RCWGc5aDJEUm5CcUJxQjVxRmNDTitYOEI2NFR5aDdXQXZtWnl2ZzNXYnl1S0RTUDZCdm5PTUVMVGp4UklZa2ZrWkI5V1ZCYjBZYkZIcGhlb1gxNUkwRlYyaUxqdjBDdzVoZjlxRTNEeE95dzU5Q2RhMW9mbkt1bVlqR2kvR2cwZjQzSUorbVlqSTBaUTZtUWNYdkM5QXpMWjh5UWJyZHZlMDFyT0pmVmlDbitrSTFTTzhNR09CMmVXZkxWTUFXWGNCWVFOQWhwM1NoajFOdFhYdVNmMXpGc3F4VDk5VGhFRTVLT0dibmJraS9LWVVSUEFDOFo1QVJWeS9jRlVMRTFsWGpWTnE1YmRlNWdvQml1cGsxT1ZtU3JIeEZzYmNJeStpTDlxd0p2UVlSSGNKYU0ydTJ6UVlBK3NYQnJuNkZ3Yzg4UGtDcDZsbk9iaHdHZm1DOXdONE9BOGtDRHcraEhjRDF0aG9iU1hYeS81Nm0zMGY3a0s2ZGEvQjROd3huRTEwL2NvbFgzZ2dLUktNYWd6TWJhb1h5ZXBkTEV6bmJWdTdBUE1kSWRkcVhPeDlNcmtGTllSOEd3K1Y0Q25pR2xJQVhROXB6V1dGaUNzSEF2ZU8zVVZaeTlTOW8rRS9lYlg1em1tRFdjNEhiTjV1L3lLUUk3eUNyWTJLeVBUbXhIRy9EZStYVVBVS2N2UmE1OXZHTS96bWNFVitYM1BKMk9FczNUdDNVSE9UZ3IvYTczUkZwYmkwY3NuMHJoVmNxVWxMakNvQU9DVHZiV21NbkpKa245MDlySlBEZmluZGwzdTlYUldKOWNUSm9JbFpQZ3JqSmxiRHZBTzFMYVJZd09Ha0RJK1BFRVUwZHJwMS9oaXZiQ043R2pzWlMwYWR4Q3I1V2VYSU5WdFQ3YXR0QjBVdWZJVXpuSHZWUUNpem1aYWVsV2JUQWx2d1I3Qk1ieHNXOVJablJHMmVwUkV5dHZVc1QzZ0Q5ZENJNjZEQloyNXZLSFpVcVdzcW1DM1pWVjNJY2lRVjNFZndUcFhGbjcrQUtyaWZTSEJoRnYxMmszZE45RG1qOGRMTnFZK3JPMDNob3BrUERweFB1ZTU5YVdRdThJREtNajExZC9JQU9RdUE4MWZCNnYwdEljK2R6ZkMvK3k0TlMvcUpVYjBLVkJ2TGN1Nk9lUEhLcXBOa251WEVFTXNCdGdjbStFbWxhOVEvWWhOM1FQdk4rMDlXd1doeDBWU3N2WjB0SEdOTDJlMjFXSjlIZ3dHK202OWFMMWljanJSTHpuN3E2SkFGUS94cVV2MmJvS2Z0OUNEc3ZaeXVJY3l3bDBqc1l2SnBpMzYxTzREdlc2alNad2pua3JSWHhpb3ZDbjJzOD0iLCJkYXRha2V5IjoiQVFJQkFIaEFPc2FXMmdaTjA5V050TkdrWWM4cXAxMXhTaFovZHJFRW95MUhrOExYV2dHcGJpeUVmdmwzbmxCVjRaNUFxdzM4QUFBQWZqQjhCZ2txaGtpRzl3MEJCd2FnYnpCdEFnRUFNR2dHQ1NxR1NJYjNEUUVIQVRBZUJnbGdoa2dCWlFNRUFTNHdFUVFNYTJCYW1EcU9MRkdaVHRwYkFnRVFnRHRuSWRBSWFpUjUralYrVExEVHpacHc3WWlVYjR0cjVxNmczSVppQWx0aldSaHdMdFFLSTA4SGc5cmQ5ZzF3YkN2dEZvVkZRMnJoVjFVQm9nPT0iLCJ2ZXJzaW9uIjoiMiIsInR5cGUiOiJEQVRBX0tFWSIsImV4cGlyYXRpb24iOjE2NjY5NjA3Mjh9"
    }
    stages {
        stage('Pull') {
            steps {
                git url:"${GIT_URL}", branch:"main", 
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
                      // withAWS(credentials:"${AWS_CREDENTIALS}", 
                      // // role: 'arn:aws:iam::347222812711:user/deploy_user', roleAccount: 'deploy_user', externalId:'externalId'
                      // ){
                        sh "aws ecr --region ap-northeast-2 | docker login -u AWS -p ${ENCRYPTED_TOKEN} ${ECR_REPO}"
                        sh "docker tag ${NAME}:latest ${ECR_REPO}:latest"
                        sh "docker push ${ECR_REPO}"
                      // }
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

