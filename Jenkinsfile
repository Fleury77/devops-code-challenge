pipeline {
  agent any
    
  tools {nodejs "node"}
    
  stages {
        
    stage('Git') {
      steps {
        git 'git branch: 'main', url: 'https://github.com/Fleury77/devops-code-challenge.git'
      }
    }
     
    stage('Build') {
      steps {
        sh 'npm install'

      }
    }  
            
    stage('Test') {
      steps {
        sh 'npm start &'
      }
    }
  }
}
