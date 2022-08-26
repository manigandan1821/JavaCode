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
        stage ('deploy') {
            steps {   
                deploy adapters: [tomcat9(credentialsId: '11', path: '', url: 'http://54.226.174.58:9090/')], contextPath: 'javaapplication', war: '**/*.war'
            }
        }
    }
}
