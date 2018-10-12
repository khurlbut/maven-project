pipeline {
	agent any
	stages {
		stage('Build') {
			steps {
				sh 'echo here!'
				sh 'mvn clean package'
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
	}
}