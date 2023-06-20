pipeline {
  agent any
  tools {
    maven 'Maven_3_5_2'
  }

  // Declare the 'app' variable at the top level
  def app

  stages {
    stage('Compile and Run Sonar Analysis') {
      steps {
        sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=myjenkinskey -Dsonar.organization=myjenkinsorg -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=37ae10f7580cdc66b4fdbd98602ff16bacb0c9cd'
      }
    }

    stage('Run SCA Analysis Using Snyk') {
      steps {
        withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
          sh 'mvn snyk:test -fn'
        }
      }
    }

    stage('Build') {
      steps {
        script {
          docker.withRegistry('https://899841823945.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
            // Assign the 'app' variable within the 'Build' stage
            app = docker.build('ajani')
          }
        }
      }
    }

    stage('Push') {
      steps {
        script {
          docker.withRegistry('https://899841823945.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
            // Access the 'app' variable defined at the top level and use it in the 'Push' stage
            app.push('latest')
          }
        }
      }
    }
  }
}
