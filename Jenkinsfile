pipeline {
	agent any

    tools {
        maven 'localMaven'
    }
	stages{
	    stage('Build') {

		    steps {
                bat 'mvn clean package'
			}
        post {
            success {
                echo 'Now archiving...'
                archiveArtifacts artifacts: '**/target/*.war'
            }
          }
		}
        stage ('Deploy to staging'){
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage ('Deploy to production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION deployment?'
                }

                build job: 'Deploy-to-Prod'
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