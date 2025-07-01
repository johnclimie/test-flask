pipeline{
  agent any
  environment{
    AWS_REGION = 'us-east-2'
    IMAGE_NAME = 'test-flask'
    REPO_NAME = 'test'
    IMAGE_TAG = 'latest'
  }
  stages{
    stage('checkout'){
      steps{
        git branch:'main', url: 'https://github.com/johnclimie/test-flask'
      }
    }
    stage('Tag the image'){
      steps{
        script{
          IMAGE_TAG = 'latest'
        }
      }
    }
    stage('Login to ECR'){
          steps{
            withAWS(region: "${env.AWS_REGION}", credentials: 'aws-creds'){
              powershell '''
              $ecrLogin = aws ecr get-login-password --region $env.AWS_REGION

              docker login --username AWS --password $ecrLogin https://451947743265.dkr.ecr.us-east-2.amazonaws.com
              '''
            }
          }
    }
    stage('Build Docker Image') {
      steps{
        powershell '''
        docker build -t $env.IMAGE_NAME:$env.IMAGE_TAG .
        docker tag 451947743265.dkr.ecr.us-east-2.amazonaws.com/test:latest
        '''
      }
    }
    stage('PUsh to ECR'){
      steps{
        powershell '''
        docker push 451947743265.dkr.ecr.us-east-2.amazonaws.com/test:latest
        '''
      }
    }
  }
}
