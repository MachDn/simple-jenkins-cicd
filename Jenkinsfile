pipeline {
  environment {
    imagename = "moveho/simple-jenkins-cicd"
    registryCredential = 'docker-credentials'
    dockerImage = ''
    repoUrl = 'https://github.com/MachDn/simple-jenkins-cicd.git'
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/MachDn/simple-jenkins-cicd.git', branch: 'master'])
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Running image') {
      steps{
        script {
          sh "docker run ${imagename}:latest"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')
            sh """
            git tag -a ${BUILD_NUMBER} -m "build number ${BUILD_NUMBER}"
            git push ${repoUrl} ${BUILD_NUMBER}
            """
          }
        }
      }
    }
  }
}
