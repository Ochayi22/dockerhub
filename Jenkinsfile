pipeline {
  agent any

  environment {
       imagename = "ochayi22/awesome"
       registryCredential = 'DockerHub'
       dockerImage = ''
           }

  tools {
    maven 'maven' 
       }

  stages {

    stage ('Build') {
      steps {
        sh 'mvn clean package'
        echo 'Maven Build'
      }
    }

    stage('Build Test') {
            steps {
                sh 'mvn test'
                echo 'Test Analysis'
            }
        }
    
    stage('Build Docker image') {
          steps{
                script {
                     dockerImage = docker.build imagename
                          }
                      }
                }

     stage('Push Docker Image to DockerHub') {
           steps{
               script {
                    docker.withRegistry(  '', registryCredential' ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                                              }
                                    }
                             }
                  }
     stage('Remove Unused docker image') {
          steps{
              sh "docker rmi $imagename:$BUILD_NUMBER"
              sh "docker rmi $imagename:latest"
                        }
            }
    
    stage ('Deploy To Tomcat Server') {
      steps{
        script {
         deploy adapters: [tomcat9(credentialsId: 'Tomcat', path: '', url: 'http://3.82.110.233:8080/')], contextPath: 'webapp', war: ' **/*.war'
           }
       }
   }
   
    stage('Deploy to QA') {
            steps {
                echo 'Deploy to Tomcat'
            }
        }        
   }
}

