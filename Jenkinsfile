pipeline {
  agent any
  environment {
    AWS_REGION = 'us-east-2'
    IMAGE_NAME = 'test-flask'
    REPO_NAME = 'test'
    IMAGE_TAG = 'latest'
  }
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/johnclimie/test-flask'
      }
    }

    stage('Login to ECR') {
      steps {
        withAWS(region: "${env.AWS_REGION}", credentials: 'aws-creds') {
          sh '''
          aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 451947743265.dkr.ecr.us-east-2.amazonaws.com
          '''
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        powershell '''
        docker build -t $env:IMAGE_NAME:$env:IMAGE_TAG .
        docker tag $env:IMAGE_NAME:$env:IMAGE_TAG 451947743265.dkr.ecr.us-east-2.amazonaws.com/$env:REPO_NAME:$env:IMAGE_TAG
        '''
      }
    }

    stage('Push to ECR') {
      steps {
        powershell '''
        docker push 451947743265.dkr.ecr.us-east-2.amazonaws.com/$env:REPO_NAME:$env:IMAGE_TAG
        '''
      }
    }
  }
}
