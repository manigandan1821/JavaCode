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
                nexusUrl: '52.90.164.246:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'App_Release', 
                version: '10.0.0'
            }
        }
	stage('E-mail Approval') {
		steps {
	    	emailext mimeType: 'text/html', subject: "[Jenkins]${currentBuild.fullDisplayName}",
	     	to: 'sm9120794@gmail.com',
	     	body: '''<a href="${BUILD_URL}input">Approval for Dev Environment</a>'''
	}
    }

        stage ('deploy') {
            steps {   
                sh 'wget --user=admin --password=admin123 http://52.90.164.246:8081/repository/App_Release/com/example/maven-project/maven-project/10.0.0/maven-project-10.0.0.war'
                deploy adapters: [tomcat9(credentialsId: '10', path: '', url: 'http://18.206.237.87:9090/')], contextPath: 'DEV', war: 'maven-project-10.0.0.war'
            }
        }
    }
}
