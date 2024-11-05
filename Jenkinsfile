pipeline {   
  agent any
  stages {
    stage("copy files to ansible server") {
      steps {
        script {
          echo "copying all necessary files to ansible control node"
          sshagent(['ansible-server-key']) {
            sh "scp -o StrictHostKeyChecking=no ansible/* root@64.226.65.202:/root"

            withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable:'user')]){
                sh 'scp $keyfile root@64.226.65.202:/root/ssh-key.pem'
            }
          }
        }
      }

    }
    stage ("execute ansible playbook") {
      steps {
        script {
          echo "calling ansible playbook to configure ec2 instances"
          def remote = [:]
          remote.name = "ansible-server"
          remote.host = "64.226.65.202"
          remote.allowAnyHosts = true

          withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable:'user')]){
            remote.user = user
            remote.identityFile = keyfile
            sshCommand remote: remote, command: "ls -l"
          }
          

        }

      }

    }           
  }
} 
