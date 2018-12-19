// pending steps: tagging git repository as it reaches an environment 
// pending steps: building UI project 

pipeline {
    agent any

    stages {    
        stage('Pre-Build') {        
        steps {
                 bat script: 'git config --global user.email "bhattacharyya.soumyak@gmail.com"'
		         bat script: 'git config --global user.name "soumyakbhattacharyya"'
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
               input(message: 'Should we deploy?', ok: 'Yes', 
                        parameters: [booleanParam(defaultValue: true, 
                        description: 'Choose yes if you want the deployment to happen',name: 'Yes?')])
                
                echo 'Deploying....'
            }
        }
        stage('run-parallel-branches') {
		  steps {
		    parallel(
		      a: {
		        echo "run performance test cases"
		      },
		      b: {
		        echo "run acceptance test cases"
		      }
		    )
		  }
		}
		stage('tag-vcs'){
		 steps {
		     bat script: 'git config --global user.email "bhattacharyya.soumyak@gmail.com'
		     bat script: 'git config --global user.name "soumyakbhattacharyya"'
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
            mail to: 'bhattacharyya.soumyak@gmail.com',
             subject: "Successful Pipeline: ${currentBuild.fullDisplayName}",
             body: "All good for ${env.BUILD_URL}"
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'I failed :('
            mail to: 'bhattacharyya.soumyak@gmail.com',
             subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
             body: "Something is wrong with ${env.BUILD_URL}"
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