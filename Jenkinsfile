pipeline {
 
    environment {
        dockerregistry = 'https://registry.hub.docker.com'
        dockerhuburl = "peswar/chitchat"
        githuburl = 'https://github.com/padieswar/chitchat.git'
        dockerhubcrd = 'dockerhub'
    }
 
    agent any
 
    tools {nodejs "node"}
 
    stages {
 
        stage('Clone git repo') {
            steps {
                git 'https://github.com/padieswar/chitchat.git'
            }
        }
 
        stage('Install Node.js dependencies') {
            steps {
                sh 'npm install'
            }
        }
 
        stage('Test App') {
            steps {
                sh 'npm test'
            }
        }
 
        stage('Build image') {
          steps{
            script {
              dockerImage = docker.build(peswar/chitchat + ":$BUILD_NUMBER")
            }
          }
        }
 
        stage('Test image') {
            steps {
                sh 'docker run -i ' + peswar/chitchat + ':$BUILD_NUMBER npm test'
            }
        }
 
        stage('Deploy image') {
          steps{
            script {
              docker.withRegistry(dockerregistry, dockerhubcrd ) {
                dockerImage.push("${env.BUILD_NUMBER}")
                dockerImage.push("latest")
              }
            }
          }
        }
 
        stage('Remove image') {
          steps{
            sh "docker rmi $peswar/chichat:$BUILD_NUMBER"
          }
        }
    }
}
