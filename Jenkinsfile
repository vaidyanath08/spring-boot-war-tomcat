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
        
        stage('Create Packer AMI') {
        steps {
          withCredentials([
            usernamePassword(credentialsId: '427eca12-088b-4675-990c-61ee33c20721', passwordVariable: 'AWS_SECRET', usernameVariable: 'AWS_KEY')
          ]) {
            sh 'packer build -var aws_access_key=${AWS_KEY} -var aws_secret_key=${AWS_SECRET} packer/packer.json'
        }
      }
    }
    
    stage('aws deployment') {
        steps {
          
            sh 'terraform init' 
            sh 'terraform plan build -var aws_access_key=${AWS_KEY} -var aws_secret_key=${AWS_SECRET} terraform/instance.tf'
        	sh 'terraform plan build -var aws_access_key=${AWS_KEY} -var aws_secret_key=${AWS_SECRET} terraform/instance.tf'
        
      }
    }
        
         /*
        stage('Deploy to Tomcat'){
            steps{
      sshagent(['tomcat-dev']) {
         sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@18.220.240.99:/opt/tomcat/apache-tomcat-8.5.34/webapps/'
      }
      }
   }
   
   */

    }
}
