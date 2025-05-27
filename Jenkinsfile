pipeline {
    agent any
 
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Lakshmikdev21/Petclinic.git'
            }
        }
          stage('Build') {
            steps {
               bat 'mvn clean install'
            }
        }
          stage('Test') {
            steps {
               bat 'mvn test'
            }
        }
          stage('Generate Juint Test Results') {
            steps {
               junit 'target/surefire-reports/*.xml'
            }
        }
          stage('Generate Artifacts') {
            steps {
               archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
            }
        }
		stage('Deploy to Tomcat Server') {
        steps {
            deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'Tomcat', path: '', url: 'http://localhost:8080')], contextPath: 'Petclinic', war: 'target/*.war'
        }
    }
    }
}