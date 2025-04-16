pipeline{

agent any

tools{
maven 'maven3.8.2'

}

triggers{
pollSCM('* * * * *')
}

options{
timestamps()
buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
}

stages{

  stage('CheckOutCode'){
    steps{
    git branch: 'main', credentialsId: 'githubcred', url: 'https://github.com/DevOpsproject-asn/Maven-Project-Testing.git'
	
	}
  }
  
  stage('Build'){
  steps{
  sh  "mvn clean package"
   }
  }
  stage('Build Docker Image') {
            steps {
                sh "docker build -t 952149495092.dkr.ecr.ap-south-1.amazonaws.com/maventestproject ."
            }
        }
}


}//Pipeline closing
