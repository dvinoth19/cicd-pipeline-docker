pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
    stage('Build Docker Image')	{
		when {
			branch 'master'
		}
		steps {
		    scripts {
			    app=docker.build("dvinoth19/node-app")
			    app.inside {
			    sh 'echo $(curl http://54.188.233.15:8081)'
			    }
			}
		}
	}
	
	stage('Push Docker Image') {
		when {
			branch 'master'
		}
		steps {
			script {
				docker.withRegistry('https://registry.hub.docker.com', 'dvinoth19') {
					app.push("${env.BUILD_NUMBER}")
					app.push("latest")
				}
			}
		}
    	}
}
}
