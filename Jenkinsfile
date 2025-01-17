pipeline {
  agent any 
  stages {
    
     stage ('Build') {
     steps {
            echo 'Running Build Automation'
            sh './gradlew build --no-daemon'
            archiveArtifacts artifacts: 'dist/trainSchedule.zip'
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
                  sh 'echo $(curl localhost:8080)'
                     }
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
                         app.push("${env.BUILDNUMBER}")
                         app.push("latest")
                         }
              }
        }

stage ('DeployToProduction') {
        when { 
                branch 'master'
             }
        steps {
                input 'Deploy to Production'
                milestone(1)
                withCredentials ([usernamePassword(CredentialsId: webserver_login, usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                        script {
                                sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@{env.prod_ip} \"docker pull ambadasu/train-schedule:{env.BUIL>
                                try {
                                        sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.prod_ip} \"docker stop train-schedule\""
                   sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.prod_ip} \"docker rm train-schedule\""
                } catch (err) {
                    echo: 'caught error: $err'
                }
                sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no   $USERNAME@${env.prod_ip} \"docker run --restart always --name train-schedule -p 8080:80>
            }
        }
    }
}
    
    
   }
}
