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

    stage ('DeployToStagin'){
      when {
        branch 'master'
      }
      steps {
        withCredentials([usernamePassword(credentialsId: 'webser_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]){
          sshPublisher{
            publishers: [
              sshPublisherDesc(
                configName: 'staging',
                sshCredentials: [
                  username: "$USERNAME",
                  encryptedPassphrase: "$USERPASS"
                ],
                transfers:[
                  sshTransfer(
                    sourceFiles: 'dist/trainSchedule.zip',
                    removePrefix:'dist/',
                    remoteDirectory: '/tmp',
                    execCommand: 'sudo rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule/'
                  )
                ]
              )
            ]
          }

        }

      }
    }
  }
}
