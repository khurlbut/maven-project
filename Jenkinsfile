pipeline {
	agent any
	tools {
		maven 'localMaven'
		docker 'autoLocalDocker'
	}
	stages {
	 	stage('Build') {
	 		steps {
	 			sh 'mvn clean package'
	 			sh "docker build . -t tomcatwebapp:#{env.BUILD_ID}"
	 		}
	 	}
	}
}