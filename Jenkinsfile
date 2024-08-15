pipeline{
   agent any
    tools {
        jdk 'java17'
        maven 'Maven3'
    }
    environment {
        APP_NAME = "prod-e2e-pip"
        RELEASE = "1.0.0"
        docker_USER = "youzarou"
        DOCKER_PASS = 'jenkins-sonar-docker'
        IMAGE_NAME = "${docker_USER}" * "/" * "${APP_NAME}"
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
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/youzarou17/prodcution-e2e-pipeline'
            }

        }

        stage("build application"){
            steps {
                sh "mvn clean package"
            }

        }
        stage("test application"){
            steps {
                sh"mvn test"
            }

        }

        stage("sonarqube analyst") {
            steps {
               script { 
                   withSonarQubeEnv(credentialsId: 'jenkins-sonar-token'){
                sh"mvn sonar:sonar"
            }
          }
        }
        } 

       stage("quality gate") {
            steps {
                script {
                waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonar-token'
            }

        }
        } 

         stage("build & push docker image") {
            steps {
                script {
                    dockerwithRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }
                    dockerwithRegistry('',DOCKER_PASS) {
                        docker_image.push "${IMAGE_TAG}"
                        docker_image.push ('latest')

            }

        }
        } 
} }

