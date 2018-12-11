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
                bat script: 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
    post {
        always {
            echo 'One way or another, I have finished'
            deleteDir() /* clean up our workspace */
        }
        success {
            echo 'I succeeeded!'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'I failed :('
        }
        changed {
            echo 'Things were different before...'
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