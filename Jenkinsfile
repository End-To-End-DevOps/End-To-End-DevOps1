pipeline {
  agent any
    tools {
      //dockerTool 'Docker'
      maven 'maven'
}
    stages {  
      stage('Code Checkout') {
         steps {
            script{
     // calculate GIT lastest commit short-hash

               gitCommitHash = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
               git rev-parse --short HEAD
               shortCommitHash = gitCommitHash.take(7)
            }
          }
     }
         
      stage('Sonar Quality Check') {
            steps {
              script{
                withSonarQubeEnv(credentialsId: 'Sonar') {
                sh 'mvn sonar:sonar'
            }
               timeout(time: 1, unit: 'HOURS') {
               def quality_gate = waitForQualityGate()
                 if (quality_gate.status != 'OK') {
                  error "Pipeline aborted due to Quality gate failure: ${quality_gate.status}"
                 }
             }
              } 
              
                  }
        }
        stage('Build maven ') {
            steps { 
                    sh 'pwd'      
                    
                    sh 'mvn  clean install package'
                  }
        }
        
        stage('Copy Artifact') {
           steps { 
                   sh 'pwd'
		   sh 'cp -r target/*.jar docker'
		   sh 'ls -altr docker'
                 }
        }
         
        stage('Build and push docker image to Registry') {
           steps {
               script { 
		             sh 'cd docker'
                 def customImage = docker.build('sibu2272/petclinicnew', "./docker")
                 docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                 customImage.push("${env.BUILD_NUMBER}")
}
           }
        }
      }
    }
}
