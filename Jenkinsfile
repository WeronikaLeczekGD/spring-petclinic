pipeline {
  environment {
    registry = "wleczek/mr"
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent any
  tools {
    maven 'Maven 3.8.6'
    jdk 'jdk8'
  }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/WeronikaLeczekGD/spring-petclinic'
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
    stage('Build') {
      steps {
        sh 'mvn package' //compilation and packaging of the code
      }
    }
    stage('Test') {
      steps {
        sh '''
        mvn clean install
        ls
        pwd
        '''
        //if the code is compiled, we test and package it in its distributable format; run IT and store in local repository
      }
    }
    stage('Create Docker Image') {
      steps {
        script {
          dockerImage = docker.build registry + ":${env.BUILD_NUMBER}"
        }
      }
    }
    stage('Push Docker Image') {
      steps {
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
            dockerImage.push("${env.BUILD_NUMBER}")
          }
        }
      }
    }

  }
}
