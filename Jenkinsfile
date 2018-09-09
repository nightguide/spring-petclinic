#!groovy

pipeline {
  agent none
  stages {
    stage('Maven Install') {
      agent {
        docker {
          image 'maven:3.5.0'
        }
      }
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t kub-ansible:5000/admin/spring-petclinic:latest .'
      }
    }

 stage('Docker Push') {
      agent any
      steps {
          sh "docker login -u admin -p N0v0sibirsk! kub-ansible:5000"
          sh 'docker push kub-ansible:5000/admin/spring-petclinic:latest'
        }
       }
 stage('Deploy to Kubernetes') {
      agent any
      steps {
          input('Do you want to processed?')
          sh 'kubectl replace -f Deployment.yml --force'
         }
        }
      }
    }
