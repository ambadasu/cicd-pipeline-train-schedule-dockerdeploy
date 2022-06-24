pipeline {
  agent any 
  stages {
     stage ('Build') {
     steps {
            echo 'Running Build Automation'
            sh './gradlew build --no-daemon'
            archiveArtifacts artifacts: 'dist/trainSchedule.zip
            }

     }

    stage('Build Docker Image') {
         when {
                branch 'master'
                }
        steps {
            script {
              app = docker.build('ambadasu/train-schedule') 
              app.inside {
                  sh 'echo $(curl localhost:8080')
                     }
                  }
        }

    

    stage ('Push Docker Image') {
        when {
        branch 'master'
        }

        steps {
               script {
                         docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-creds')
                         app.push("${env.BUILDNUMBER}}")
                         app.push("latest")
                         }
              }
        }


   }
}
