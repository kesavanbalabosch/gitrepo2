pipeline {
  agent any
  stages {
    stage('SCM') {
      steps {
        git(url: 'https://github.com/kesavanbalabosch/gitrepo2', credentialsId: 'dc29dae3-35d4-4e0f-82b8-e136ad2d0d9f', branch: 'main')
      }
    }

    stage('Build&Publish') {
      steps {
        sh '''sudo docker build -t kesavanbalabosch/srepo1:latest .
sudo docker push kesavanbalabosch/srepo1:latest'''
      }
    }

    stage('Deploy') {
      parallel {
        stage('DeployDev') {
          steps {
            sh '''sudo docker rm -f $(sudo docker ps -aq)||true
sudo docker run -d -p 7777:80 --name con2 kesavanbalabosch/srepo1:latest'''
          }
        }

        stage('DeployQA') {
          steps {
            sh '''sudo docker rm -f $(sudo docker ps -aq)||true
sudo docker run -d -p 7777:80 --name devcon2 kesavanbalabosch/srepo1:latest'''
          }
        }

      }
    }

  }
}