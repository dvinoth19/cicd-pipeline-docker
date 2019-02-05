pipeline {
    agent any
	tools {
		jdk 'java1.8.0'
		gradle 'gradle'
    	}
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
    stage('Build Docker Image')	{
		steps {
		    script {
			    app = docker.build("dvinoth19/node-app")
			    app.inside {
			    sh 'echo $(curl localhost:8081)'
			    }
			}
		}
	}
	
	stage('Push Docker Image') {
		steps {
			script {
				docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
					app.push("${env.BUILD_NUMBER}")
					app.push("latest")
				}
			}
		}
    	}
}
}
