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
	    stage('SonarQube Analysis') {
            steps {
              withSonarQubeEnv('sonar') {
                 sh "mvn clean verify sonar:sonar"
                 }
             }
         }
		stage('Execute Sonarqube Report')
		{
			steps 
			{
			   withSonarQubeEnv('sonar')
			    {
			      sh "mvn sonar:sonar"
                            }
                        {
                 }	
		stage('Quality Gate Check')
		{
		     steps
		     {
			timeout(time: 1, unit: 'Hours')
			{
			    waitForQualityGate abortPipeline: true
			}
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
