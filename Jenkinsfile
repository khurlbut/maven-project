pipeline {
	agent any
	environment {
    	PATH = "/Users/ke015t7/development/apache-maven-3.5.4/bin:$PATH"
  	}
	stages {
		stage('Build') {
			steps {
				sh 'echo PATH=$PATH'
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