pipeline {
	agent any

	tools {
    	maven 'localMaven'
  	}

  	parameters {
  		string(name: 'tomcat-staging', defaultValut: '18.191.99.142', description: 'Staging Server')
  		string(name: 'tomcat-production', defaultValut: '18.191.99.142', description: 'Production Server')
  	}

  	triggers {
  		pollSCM('* * * * *')
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

		stage('Deployments') {
			parallel {

				stage('Deploy to Staging') {
					steps {
						sh 'scp /Users/ke015t7/Documents/learning/jenkinspipeline/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat-staging}:/var/tomcat8/webapps'
					}
				}

				stage('Deploy to Production') {
					steps {
						timeout(time:5, unit:'DAYS') {
							input message:'Approve PRODUCTION Deployment?'
						}

						sh 'scp /Users/ke015t7/Documents/learning/jenkinspipeline/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat-production}:/var/tomcat8/webapps'
					}
					post {
						success {
							echo 'Code deployed to Production.'
						}

						failure {
							echo 'Deployment failed.'
						}
					}
				}	

			}
		}
	}
}