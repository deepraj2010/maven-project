pipeline {
	agent any
	stages{
		stage('Build'){
			steps{
				sh 'mvn clean package'
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
		stage('Deploy to Staging'){
		// This is a function in jenkin
			build job: 'Deploy-to-staging'
		}
		
		stage('Deploy to Production'){
			steps{
				timeout(time:5, unit:'DAYS'){
					input message: 'Approve Production deployment', submitter: admin
				}
				
				build job: 'Deploy-to-production'
			}
			post {
				success {
					echo 'Code deployed to production'
				}
				
				failure {
					echo 'Deployment failed'
				}
			}
		}	
	}
}