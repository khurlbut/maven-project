pipeline {
	agent any
	environment {
		DOCKER="/Applications/Docker.app/Contents/Resources/bin"
	}
	tools {
		maven 'localMaven'
	}
	stages {
	 	stage('Build') {
	 		steps {
	 			sh 'mvn clean package'
	 			sh "$DOCKER/docker build . -t tomcatwebapp:${env.BUILD_ID}"
	 		}
	 	}
	}
}
