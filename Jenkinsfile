pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    tools {
        maven 'maven-3.8.1'
        jdk 'jdk17'
        nodejs 'node-16'
    }

    stages {
        stage('Build & Test backend') {
            steps {
                dir("backend") {
                    sh 'mvn package'
                }
            }

            post {
                success {
                    junit 'backend/target/surefire-reports/**/*.xml'
                }
            }
        }

        stage('Build frontend') {
            steps {
                dir("frontend") {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }
        
        stage('Save artifacts') {
            steps {
                archiveArtifacts(artifacts: 'backend/target/sausage-store-0.0.1-SNAPSHOT.jar')
                archiveArtifacts(artifacts: 'frontend/dist/frontend/*')
            }
	post {
	    success {
			sh "curl -X POST -H 'Content-type: application/json' --data '{\"chat_id\": \"893313228\", \"text\": \"Приложение успешно собрано.\" }' https://api.telegram.org/bot7614166167:AAGNZvZX3e9PnrMtwiPMgRXa9BgbULazbRs/sendMessage"		
	   }
	}
}
}
} 
