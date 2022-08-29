pipeline {
    agent any
    tools {
    maven 'M2'
    }
	try {
	   stage("Building SONAR ...") {
	sh './gradlew clean sonarqube'
	}
		} catch (e) {emailext attachLog: true, body: 'See attached log', subject: 'BUSINESS Build Failure', to: 'abc@gmail.com'
	step([$class: 'WsCleanup'])
	return
}
  stages {
        stage('checkout') {
            steps {
                git 'https://github.com/manigandan1821/JavaCode.git'
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean install'
            }
        }

	stage('SonarQube Analysis') {
        steps {
              withSonarQubeEnv('sonar') {
                 sh "mvn clean verify sonar:sonar"
                 }
             }
	}
	stage('Execute Sonarqube Report')  {
          steps {
             withSonarQubeEnv('sonar') {
                 sh "mvn sonar:sonar"
              }  
          }
      }
      stage('Quality Gate Check') {
          steps {
              timeout(time: 1, unit: 'HOURS') {
                 waitForQualityGate abortPipeline: true
             }
          }
      }
    }
} 
