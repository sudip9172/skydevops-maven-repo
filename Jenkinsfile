pipeline {
  environment {
    VERSION = "${env.BUILD_ID}"
    dockerimagename = "ravirekha1/skydevopsapp"
    registry = "https://hub.docker.com/"
    registryCredential = 'my-docker-private-id'
    dockerImage = ""	  
	  
    }
	tools{
        maven 'maven3.9.0'
    }
  agent {
  label 'my_slave1'
  }
  stages{
    stage ('Checkout SCM') {
          steps {
			 git branch: 'main', url: 'https://github.com/ravirekha/skydevops-maven-repo.git'
            }
        }    
	
	stage ('Build Package') {

            steps {
               sh 'mvn clean package'
            }
        }
	stage('SonarQube analysis') {
        steps{
          withSonarQubeEnv('Sonar-server') { 
             sh "mvn sonar:sonar"
            }
          }
		}

    stage('Building image') {
      steps{
        script {
         
          dockerImage = docker.build dockerimagename
       }
      }
    }
    stage('Pushing Image') {
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
              dockerImage.push("${env.BUILD_NUMBER}")
  //          dockerImage.push("latest")
             
          }
        }
      }
    }
  }
}
