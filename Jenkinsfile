pipeline{
    agent any
    tools{
        maven 'maven3'
    }
    environment {
        APP_NAME = "devops-integration"
        RELEASE = "1.0.0"
        DOCKER_USER = "dheerdwivedi"
        DOCKER_PASS = "dockerhub"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    
    stages{
        stage('Clean Workspace'){
            steps{
                cleanWs()
            }
        }
        stage ('Checkout from SCM'){
            steps{
                git branch: 'main', credentialsId: 'mygithubtokenid', url: 'https://github.com/dheerd/devops-automation'
            }
        }
        stage('Build project'){
            steps{
               bat 'mvn clean install'
            }
        }
        stage("Unit Testing"){
            steps{
                bat 'mvn test'
            }
        }
        
        stage('Build Docker Image'){
            steps{
                //bat 'docker build -t dheerd/devops-integration .'
                script{
                    docker.withRegistry('', DOCKER_PASS){
                        docker_image = docker.build "${IMAGE_NAME}"
                    }
                }
            }
        }
        
        stage ('Push Docker Image to Docker  Hub'){
            steps{
               script{
                   docker.withRegistry('', DOCKER_PASS){
             
                       docker_image.push("${IMAGE_TAG}")
                       docker_image.push("latest")
             
                   }
              }
            }
        }
        
    } 
}