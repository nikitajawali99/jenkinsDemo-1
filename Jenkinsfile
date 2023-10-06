pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
              git url: 'https://github.com/nikitajawali99/jenkinsDemo-1.git'      
		            echo "Code Checked-out Successfully!!";
            }
        }
        
        stage('Package') {
            steps {
              dir("${env.WORKSPACE}/JenkinsDemo1"){
                bat 'mvn package'    
		            echo "Maven Package Goal Executed Successfully!";
              }
            }
        }
        
        stage('JUNit Reports') {
            steps {
               dir("${env.WORKSPACE}/JenkinsDemo1"){
                    junit 'target/surefire-reports/*.xml'
		                echo "Publishing JUnit reports"
               }
            }
        }
        
        stage('Jacoco Reports') {
            steps {
                dir("${env.WORKSPACE}/JenkinsDemo1"){
                  jacoco()
                  echo "Publishing Jacoco Code Coverage Reports";
                }
            }
        }

	stage('SonarQube analysis') {
            steps {
               
		// Change this as per your Jenkins Configuration
		 dir("${env.WORKSPACE}/JenkinsDemo1"){
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn package sonar:sonar'
                }
		     } 
            }
        }

        
    }
    post {
        
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
    
    }
}
