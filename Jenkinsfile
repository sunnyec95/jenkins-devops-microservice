//SCRIPTED

//DECLARATIVE
pipeline {
	agent any
	//agent { docker { image 'maven:3.6.3'} }
	
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}

	stages {
		stage('Checkout') {
			steps {
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "Build_ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "Build_TAG - $env.BUILD_TAG"
				echo "Build_URL -$env.BUILD_URL"
			}
		}
		stage('Compile') {
			steps {
				sh "mvn clean compile"
			}
		}		
	    
		stage('Test') {
		    steps {
				sh "mvn test"
			}
		} 
		
		stage('Integration Test') {
			steps {
				sh "mvn failsafe:integration-test failsafe:verify"
			}
		}	
		stage('Package') {
			steps {
				sh "mvn package -DskipTests"
			}
		}	
		stage('Build Docker Image') {
			steps {
				//"docker build -t sunnyec95/currency-exchnage-devops:$env.BUILD_TAG"
				script {
					dockerImage = docker.build("sunnyec95/currency-exchnage-devops:${env.BUILD_TAG}")
				}
			}
		}
	    stage('Push Docker Image') {
			steps {
				script {
					docker.withRegistry ('',dockerhub) {
						dockerImage.push();
						dockerImage.push('latest');


					}
						
				}
			}
		}
	
	post {
		always {
			echo 'im awesome. I run always'
		}
		success {
			echo ' I run when you are successful'
		}
		failure {
			echo '. I run when you failed'
		}
	}
}
