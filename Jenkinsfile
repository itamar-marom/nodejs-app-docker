pipeline {
  environment {
    // Docker registry information, please replace 'docker_hub_account' with your own.
    registry = "docker_hub_account/nodejs-app-docker"
    registryCredential = 'dockerhub'
    // create an environment to save docker image informations
    dockerImage = ''
  }

  agent any

  stages {
    // Build ing the docker image. It will run the docker build and use the jenkins build number in docker tag.
    // With build number turn easeful to deploy or rollback based in jenkins.
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    // Push the docker image builded to dockerhub.
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    // After build and deploy, delete the image to cleanup your server space.
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}