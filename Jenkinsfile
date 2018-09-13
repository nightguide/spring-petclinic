#!groovy


  
pipeline {
  agent none
  
  notifyStarted()
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
    
      stage('Unit Tests') {
      agent any
      steps {
       junit 'target/surefire-reports/*.xml'
 
      }
    }
  
  stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t kub-ansible:5000/admin/spring-petclinic:$BUILD_NUMBER .'
      }
    }
 stage('Docker Push') {
      agent any
      steps {
          sh "docker login -u admin -p N0v0sibirsk! kub-ansible:5000"
          sh 'docker push kub-ansible:5000/admin/spring-petclinic:$BUILD_NUMBER'
        }
       }
 stage('Deploy to Kubernetes') {
      agent any
      steps {
          input('Do you want to processed?')
           sh 'kubectl set image deployment/spring-petclinic spring-petclinic=kub-ansible:5000/admin/spring-petclinic:$BUILD_NUMBER -n=dev'
         }
        }
      }
    }
def notifyStarted() {
  
  emailext (
      subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
      recipientProviders: [[$class: 'DevelopersRecipientProvider']]
    )
}
