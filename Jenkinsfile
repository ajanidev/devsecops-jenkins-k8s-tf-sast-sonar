pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=myjenkinskey -Dsonar.organization=myjenkinsorg -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=37ae10f7580cdc66b4fdbd98602ff16bacb0c9cd'
			}
        } 
  }
}
