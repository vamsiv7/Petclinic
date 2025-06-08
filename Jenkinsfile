pipline {

    tools{
         
         maven 'maven'
    }
    agent any
 
    stages {
        stage('Clone') {
            steps {
                git branch: 'release/2025.06.08', url: 'https://github.com/vamsiv7/Petclinic.git'
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
 	      -Dsonar.token=sqp_8edf4fe84caf6d659351a7a9091e056d5cca26cb
            }
        }


       stage('Artifactory Server') {
          steps {
              rtServer (
               id: 'Artifactory",
                url:'http://localhost:8081/artifactory
                 username:'admin',
                  password: 'password',
                  bypassProxy: true
                  timeout: 300
                  )
       }
 }

       stage('Upload') {
           steps {
          	 rtDownload (
        	 serverId: 'Artifactory',
        	 spec: '''{
          	"files": [
            	  {
        	  "pattern": "*.war",
         	  "target": "SRinfotech-private-limited",
           	  }
                 	 ]
                  	}''', 
                	  )
       }                              
 }

   stage('Publish Build info') {
     steps {
         rtPublishBuildInfo (
          serverId: 'Artifactory',  
        )
          
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
   }
}
