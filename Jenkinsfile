pipeline {
    agent any 
    stages {
        stage('clone and clean') { 
            steps {
                sh 'mvn clean'
            }
        }
        stage('Test') { 
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') { 
            steps {
                sh 'mvn package'
            }
        }
        stage('Deploy to Tomcat'){
            steps{
      sshagent(['tomcat-dev']) {
         sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@18.220.240.99:/opt/tomcat/apache-tomcat-8.5.34/webapps/'
      }
      }
   }

    }
}
