pipeline{
   agent any
    tools {
        jdk 'java17'
        maven 'Maven3'
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

} }

