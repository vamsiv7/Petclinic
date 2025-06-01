pipeline {
    agent any
 
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/vamsiv7/Petclinic.git'
            }
        }
          stage('Build') {
            steps {
               bat 'mvn clean install'
            }
        }
        stage('SonarQube Server') {
            steps {
                bat 'mvn install'
              bat '''mvn sonar:sonar\
         -Dsonar.projectKey=Petclinic \
         -Dsonar.projectName='Petclinic' \
         -Dsonar.host.url=http://localhost:9000 \
         -Dsonar.token=sqp_53efb685409b588423820400938fced5a343dbe0'''
            }
        }
          stage('Generate Artifacts') {
            steps {
               archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
            }
        }
		stage('Deploy to Tomcat Server') {
        steps {
            deploy adapters: [tomcat9(alternativeDeploymentContext: '', path: '', url: 'http://localhost:9090')], contextPath: 'Sonarqube', war: 'target/*.war'
    }
}
