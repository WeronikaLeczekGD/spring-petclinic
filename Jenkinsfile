pipeline {
  environment {
    registry = "wleczek/mr"
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent any
  tools {
    maven 'mymaven'
    jdk 'Myjava'
    dockerTool 'mydocker'
  }
  stages {
    stage('Cloning Git') {
      steps {
        git branch: 'main', credentialsId: 'github-mr-user1', url: 'https://github.com/WeronikaLeczekGD/spring-petclinic'
      }
    }
  stage('Checkstyle') {
    steps {
       sh "mvn checkstyle:checkstyle"}
       }
    stage('Compile') {
       steps {
         sh 'mvn compile' //only compilation of the code
       }
    }
    stage('Test')
    {
      steps {
        sh 'mvn test'
      }
    }
    stage('Build') {
          steps {
            sh 'mvn package' //compilation and packaging of the code
          }
    }
    stage('Docker Build') {
      steps {
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        script {
          docker.withRegistry('https://hub.docker.com/repository/docker/wleczek/mr', 'dockerhub-mr-user1') {
            dockerImage.push()
            dockerImage.push("${env.BUILD_NUMBER}")
          }
        }
      }
    }
  }
}
