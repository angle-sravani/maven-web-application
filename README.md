java end to end project.
jenkins script.
pipeline{
    agent any
    tools{
        maven 'maven-1'
    }
    stages{
        stage("checkout"){
            steps{
               git 'https://github.com/angle-sravani/maven-web-application.git' 
            }
        }
       stage("build"){
           steps{
               sh "mvn clean package"
            }
      }
        stage("code quality check"){
            steps{
                 sh  "mvn clean sonar:sonar"
            }
       }
     stage("UploadArtifactsIntoNexu"){
            steps{
                sh "mvn clean deploy "
            }
        }
         stage ('docker') {
             steps {
                sh 'docker build -t 572869942318.dkr.ecr.ap-southeast-1.amazonaws.com/docker-ecr:1 .'
//       //   sh ' aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 572869942318.dkr.ecr.ap-southeast-1.amazonaws.com'
//   //sh 'docker push 572869942318.dkr.ecr.ap-southeast-1.amazonaws.com/docker-ecr:1'
            }
        }
        
        stage ('deploy to kubernetes cluster'){
            steps{
                sh 'kubectl apply -f mavenwebappdeployment.yaml'
            }
        }
    }
    
}    
 

    
