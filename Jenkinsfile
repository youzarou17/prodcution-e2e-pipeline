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

} }

