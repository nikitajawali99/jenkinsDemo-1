pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
              git url: 'https://github.com/nikitajawali99/jenkinsDemo-1.git'      
		            echo "Code Checked-out Successfully!!";
            }
        }
        
          stage('OWASP Dependency Check') {
            steps {
              dir("${env.WORKSPACE}/JenkinsDemo1"){
                
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DC'
                   dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                
              }
            }
        }
        
        stage('Compile') {
            steps {
              dir("${env.WORKSPACE}/JenkinsDemo1"){
                bat 'mvn compile'    
		            echo "Maven Compile Goal Executed Successfully!";
              }
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


      stage('Deployment') {
            steps {
              dir("${env.WORKSPACE}/JenkinsDemo1"){
             
		            echo "Maven Package Goal Executed Successfully!";
		             deploy adapters: [tomcat9(url: 'http://localhost:9090/', credentialsId: 'Tomcat-cred')],war: '**/*.war',
                     contextPath: 'jenkinsTest'
              }
            }
        }
        
    }
    post {
        
        success {
            
         mail bcc: '', body: '''echo "Build id is - $BUILD_ID"
         echo "Build URL is - $BUILD_URL"''', cc: 'nikitajawali06@gmail.com',
         from: '', replyTo: '', subject: '${currentBuild.result}', to: 'nikitajawali99@gmail.com'
            
         echo 'This will run only if successful'
         
        }
        failure {
            
            echo 'This will run only if failed'
        }
    
    }
}
