pipeline {
    agent any
    tools {
    maven 'M2'
    }

    stages {
        stage ('Build') {
            steps {
                git 'https://github.com/manigandan1821/JavaCode.git'
                sh 'mvn clean package'
            }
        }
     try {
	stage("Building SONAR ...") {
	    sh './gradlew clean sonarqube'
	}
		} catch (e) {emailext attachLog: true, body: 'See attached log', subject: 'BUSINESS Build Failure', to: 'abc@gmail.com'
	step([$class: 'WsCleanup'])
	return
    }
        stage('Upload to nexus') {
	      steps {
              nexusArtifactUploader artifacts: [
                  [
                      artifactId: 'maven-project', 
                      classifier: '', 
                      file: 'webapp/target/webapp.war', 
                      type: 'war'
                    ]
                ], 
                credentialsId: 'admin', 
                groupId: 'com.example.maven-project', 
                nexusUrl: '18.212.225.158:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'App_Release', 
                version: '1.0.0'
            }
        }
        stage ('deploy') {
            steps {   
                deploy adapters: [tomcat9(credentialsId: '11', path: '', url: 'http://54.226.174.58:9090/')], contextPath: 'javaapplication', war: '**/*.war'
            }
        }
    }
}
