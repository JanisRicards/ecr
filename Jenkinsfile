pipeline {
  agent any
  stages {
    stage ('Build') {
      steps {
        sh 'printenv'
        sh 'docker build -t ecr-jenkins .'
      }
    }
    stage ('Publish ECR') {
      steps {
        withEnv (["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
          sh 'docker login -u AWS -p $(aws ecr-public get-login-password --region us-east-1) public.ecr.aws/k6t4t9s5'
          sh 'docker build -t ecr-jenkins .'
          sh 'docker tag ecr-jenkins:latest public.ecr.aws/k6t4t9s5/ecr-jenkins:""$BUILD_ID""'
          sh 'docker push public.ecr.aws/k6t4t9s5/ecr-jenkins:""$BUILD_ID""'
        }
      }
    }
  }
}
