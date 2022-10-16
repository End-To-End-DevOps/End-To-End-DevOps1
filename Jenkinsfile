pipeline {
  agent any
    tools {
      dockerTool 'Docker'
      maven 'maven'
}
    stages {      
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
         
        stage('Build docker image') {
           steps {
               script { 
		             sh 'cd docker'
                 //def customImage = docker.build('sibu2272/petclinicnew')
                 sh 'pwd'
                 docker build sibu2272/petclinicnew
           }
        }
      }
    }
}
