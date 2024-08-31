pipeline {
    agent { label 'Jenkins-agent' }
    tools {
        jdk 'Java17'
        maven 'maven3'
    }
	environment {
	    APP_NAME = "register-app1-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "monika-035"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	    
    }

   
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

    stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/monika-035/register-app1.git'
                }
        }

    stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

        }

    stage("Test Application"){
           steps {
                 sh "mvn test"
           }
      }
        
        stage("SonarQube Analysis"){
           steps {
	           script {
		           withSonarQubeEnv(credentialsId: 'Jenkins-sonarqube-token') {
                       sh "mvn sonar:sonar"
		               }
	            }	
            }
	}
		stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
       }

         }

    }


        
    
           
