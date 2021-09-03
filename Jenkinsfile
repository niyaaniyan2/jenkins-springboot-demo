pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="747674092094"
        AWS_DEFAULT_REGION="ap-southeast-1"
        IMAGE_REPO_NAME="ecr-sep3"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
  
    stages {   
      stage('Logging into AWS ECR') {
            steps {
                script {
                sh "docker login --username AWS --password-stdin -p $(aws ecr get-login-password --region ap-southeast-1) 747674092094.dkr.ecr.ap-southeast-1.amazonaws.com"  
                }
                
            }
        }
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/niyaaniyan2/jenkins-springboot-demo.git']]])    
            }
        }
 
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        }
      }
    }
  
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{
         script {
                sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
         }
        }
      }
    }
}
