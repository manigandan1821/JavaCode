pipeline {
    agent any
    tools {
    maven 'M2'
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
                 waitForQualityGate abortPipeline: true
          }
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
                version: '2.0.0'
            }
        }
        stage ('deploy') {
            steps {   
                deploy adapters: [tomcat9(credentialsId: '11', path: '', url: 'http://54.237.200.106:9090/')], contextPath: 'javaapplication', war: '**/*.war'
            }
        }
    }
}
