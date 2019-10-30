pipeline{
  agent any 
  stages{
    stage('Git'){
      steps{
        git 'https://github.com/AlissonMMenezes/Terminus'
      }   
    }   
    stage('SonarQube'){
      environment {
        scanner = tool 'scanner'
      }   
      steps{
        withSonarQubeEnv('sonarqube'){
          sh "${scanner}/bin/sonar-scanner -Dsonar.projectKey=jasontodd -Dsonar.sources=${WORKSPACE} -Dsonar.projectVersion=${BUILD_NUMBER}"    
        }
      }   
    }   
    stage('Quality Gate'){
      steps{
        waitForQualityGate abortPipeline: true
      }   
    }   
  }
  post {
	always {
		chuckNorris()
	}
  }
}
