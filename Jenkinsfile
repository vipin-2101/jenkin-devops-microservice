pipeline{
	agent any
	//agent { docker { image 'maven:3.6.3'} }
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages{
		stage('Checkout'){
			steps{
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
				
			}
		}
		stage('Compile'){
			steps{
				sh "mvn clean compile"
			}
		}
		stage('Test'){
			steps{
				
				sh "mvn test"
				
			}
		}
		stage('Integration Test'){
			steps{
				
				sh "mvn failsafe:integration-test failsafe:verify"
			}
		}


		stage('Package'){
			steps{
				
				sh "mvn package -DskipTests"
			}
		}

		stage('Build Docker Image'){
			steps{
				script{
					dockerImage = docker.build("vipinjain/currency-exchange-devops:${env.BUILD_TAG}")
				}
			}

		}

		stage('Push Docker Image'){
			steps{
				script{
					docker.withRegistry('','dockerhub'){
						dockerImage.push();
						dockerImage.push('latest');
					}
					
				}

			}
		}
	}
}