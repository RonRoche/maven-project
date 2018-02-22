pipeline {
	agent any
	stages {
		stage ('Build') {
			steps {
				build job: 'package'
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/*.war'
				}
			}
		}
		stage ('Staging & Code Coverage') {
			parallel {
				stage ('Deploy to Staging') {
					steps {
						build job: 'deploy-to-staging'
					}
				}
				stage ('Code coverage') {
					steps {
						build job: 'static-analysis'
					}
				}
			}
		}
		stage ('Deploy to Production') {
			steps {
				build job: 'deploy-to-prod'
			}
		}
	}
}