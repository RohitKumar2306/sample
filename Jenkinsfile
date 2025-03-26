pipeline {
  agent any

  stages {
    stage("Validate Application") {
      steps {
        echo "This is build stage"
        sh "mvn validate"
      }
    }

    stage("Package the Application") {
      steps {
        echo "This is packaging stage"
        sh "mvn clean package"
      }
    }
  }
  post {
      success {
        echo "BUILD IS SUCCESSFULL"
        archiveArtifacts artifacts: '**/target/*.war'
      }
  }
}