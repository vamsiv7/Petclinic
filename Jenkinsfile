pipeline{

    agent any

    stages{

       stage('clone the project'){
        steps{
            
           git branch: 'main', url: 'https://github.com/jaiswaladi246/Petclinic.git'
          
        }

       }

       stage('Build the project'){
        steps{
           sh 'mvn clean install'
        }

       }

       stage('Test'){
        steps{
           sh 'mvn test'
        }

       }
       stage('published the test results'){
        steps{
           junit 'target/surefire-reports/*.xml'
        }

       }
       stage('publishedd the artifacts'){
        steps{
           archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
        }

       }

       stage('Deploy to Tomcat Server'){
        steps{
           deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://localhost:8080/')], contextPath: 'Ifocus Solutions Pvt Limited', war: 'target/*.war'
        }

       }


    }
}