pipeline {
  agent any

  tools {
    jdk "jdk17"
    maven "M3"
  }
  /std06-spring-petclinic:latest
  enviroment {
      AWS_CREDENTIAL_NAME ="AWSCredentials"
      REGION = "ap-northeast-2"
      DOCKER_IMAGE_NAME = "std06-spring-petclinic"
      ECR_REPOSITORY = "257307634175.dkr.ecr.ap-northeast-2.amazonaws.com"
      ECR_DOCKER_IMAGE = "${ECR_REPOSITORY}/${DOCKER_IMAGE_NAME}"
  stages {
    stage ('Git clone') {
      steps {
        echo 'Git clone'
        git url: 'https://github.com/mingyu1128/spring-petclinic.git',
            branch: 'efficient-webjars'
      }
      post {
        success {
          echo 'Git clone Success!!'
        }
        failure {
          echo 'Git clone fail'
        }
      }
    }
   
    stage ('Maven Build') {
      steps {
        echo 'Maven Build'
        sh 'mvn -Dmaven..testfailure.ignore=true clean package'
      }
      post {
        success{
          junit 'target/surefire-reports/**/*.xml'
        }
      }
    }

    stage ('Docker Image Build') {
      steps {
        echo 'Docker Image Build'
        dir("${env.WORKSPACE}") {
          sh """
            docker build -t $ECR_DOCKER_IMAGE:$BUILD_NUMBER .
            docker tag $ECR_DOCKER_IMAGE:$BUILD_NUMBER $ECR_DOCKER_IMAGE:latest
          """
        }
      }
    }
  }
}
