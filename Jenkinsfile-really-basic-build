pipeline {
    agent any

    stages {    
        stage('Pre-Build') {        
        steps {
                printBaseInfo();
            }
        
        }    
        stage('Build') {
            steps {
                 bat script: 'mvn -B -DskipTests clean install' 
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }   
 }

 def printBaseInfo(){
  echo 'Validating pre condition for build..'
  bat script: 'java -version'
  bat script: 'javac -version'
  bat script: 'npm --version'
  bat script: 'node --version'
 }