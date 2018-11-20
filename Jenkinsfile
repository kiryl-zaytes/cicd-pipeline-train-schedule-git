pipeline {
  agent any
  stages {
    stage ('Build'){
      steps {
        echo "stage build"
        sh './gradlew build --no-daemon'
        archiveArtifacts artifacts: 'dist/trainSchedule.zip'
      }
    }
  }
}
