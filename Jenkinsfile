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
        stage('SonarQube Server') {
            steps {
              sh"mvn clean verify sonar:sonar\
  -Dsonar.projectKey='Petclinic'\
  -Dsonar.projectName='Petclinic'\
  -Dsonar.host.url='http://localhost:9000'\
  -Dsonar.token=sqp_2cca513fa1211759366913d7a715b19541debb77" 
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
