pipeline {
	agent any
	tools {
		go 'gotest'
	}
	environment {
		GO111MODULE='on'
	}
 
	stages {
		stage('Test') {
			steps {
				git 'https://github.com/databinaries001/ci-cd-demo.git'
				sh 'go test ./...'
			}
		}
		stage('Build') {
			steps {
				git 'https://github.com/databinaries001/ci-cd-demo.git'
				sh 'go build .'
			}
		}
		stage('Run') {
			steps {
				sh 'cd /var/lib/jenkins/workspace/full-cicd-go && go-webapp-sample &'
			}
		}
	}
}

post {
    	success {
            script {
                // Notify success to Slack
                slackSend channel: '#spark-alerts', color: 'good', message: "Build SUCCESSFUL: ${env.JOB_NAME} [${env.BUILD_NUMBER}] (${env.BUILD_URL})"
            }
        }
        failure {
            script {
                // Notify failure to Slack
                slackSend channel: '#spark-alerts', color: 'danger', message: "Build FAILED: ${env.JOB_NAME} [${env.BUILD_NUMBER}] (${env.BUILD_URL})"
            }
        }
        always {
            script {
                // Stop and remove containers after the job
                sh 'docker-compose down'
            }
        }
    }
}
