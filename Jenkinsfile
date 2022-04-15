pipeline { 
    agent any 
    environment {
        AWS_ECR_LOGIN = 'true'
        AWS_ACCOUNT_ID="627552558471"
        AWS_DEFAULT_REGION="us-east-1" 
        IMAGE_REPO_NAME="springboot-pipeline"
        IMAGE_TAG="springboot-pipeline"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    } 
    stages { 
        stage('Logging into AWS ECR') {
            steps {
                withAWS(credentials: 'aws-key', region: 'us-east-1'){

                    sh """
                        aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com
                    """
                } 
            }
        }
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/ht-cicd-demo/HelloSpringEKS']]])     
            }
        }
        stage('Build') { 
            steps { 
                withDockerContainer(image: 'maven') {
                    sh """
                        mvn install
                        
                    """
                }
                docker build . -t ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}
            }
        }
        stage('Pushing to ECR') {
            steps {  
                withAWS(credentials: 'aws-key', region: 'us-east-1'){
                            // sh """docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"""
                            sh """docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"""
                }
            }
      }
    }
}