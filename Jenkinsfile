pipeline{
  agent any
  tools {
        maven 'MAVEN_HOME'
  }
  stages {
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace... */
      steps {
        checkout scm
      }
    }
    stage('Build and Generate Docker Images') {
      steps {
        sh 'mvn -B -DskipTests clean package'
        sh 'echo $USER'
        sh 'echo whoami'
      }
    }
    stage('Push images to aws eks'){
          steps {
            withDockerRegistry(credentialsId: 'ecr:us-west-2:aws-cred', url: 'http://182355204495.dkr.ecr.us-west-2.amazonaws.com/account-service') {
             sh 'docker tag bank-service:latest 182355204495.dkr.ecr.us-west-2.amazonaws.com/bank-service'
             sh 'docker push 182355204495.dkr.ecr.us-west-2.amazonaws.com/bank-service'

             sh 'docker tag branch-service:latest 182355204495.dkr.ecr.us-west-2.amazonaws.com/branch-service'
             sh 'docker push 182355204495.dkr.ecr.us-west-2.amazonaws.com/branch-service'

             sh 'docker tag customer-service:latest 182355204495.dkr.ecr.us-west-2.amazonaws.com/customer-service'
             sh 'docker push 182355204495.dkr.ecr.us-west-2.amazonaws.com/customer-service'

             sh 'docker tag account-service:latest 182355204495.dkr.ecr.us-west-2.amazonaws.com/account-service'
             sh 'docker push 182355204495.dkr.ecr.us-west-2.amazonaws.com/account-service'

             sh 'docker tag transaction-service:latest 182355204495.dkr.ecr.us-west-2.amazonaws.com/transaction-service'
             sh 'docker push 182355204495.dkr.ecr.us-west-2.amazonaws.com/transaction-service'
            }
          }
    }
        stage('Run docker images on kubernetis cluster') {
          steps {
            node('eks'){
              checkout scm
             sh 'git checkout feature-1.1'
              sh '/home/ec2-user/bin/kubectl apply -f deployment.yaml'
             sh '/home/ec2-user/bin/kubectl apply -f service.yaml'
            }
          }
        }  
    
  }
}
         
  
