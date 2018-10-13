pipeline {
	agent any

	tools {
    	maven 'localMaven'
  	}

  	parameters {
  		string(name: 'tomcat_staging', defaultValue: 'aws-tomcat-staging:', description: 'Staging Server')
  		string(name: 'tomcat_production', defaultValue: '18.191.99.142', description: 'Production Server')
  	}

  	triggers {
  		pollSCM('* * * * *')
  	}

	stages {
		stage('Build') {
			steps {
				sh 'echo PATH=$PATH'
				sh 'whoami'
				sh 'pwd'
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
						sh "scp -i /Users/Shared/Jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_staging}:/var/lib/tomcat8/webapps"
					}
				}

				stage('Deploy to Production') {
					steps {
						timeout(time:5, unit:'DAYS') {
							input message:'Approve PRODUCTION Deployment?'
						}

						sh "scp -i /Users/Shared/Jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_production}:/var/lib/tomcat8/webapps"
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