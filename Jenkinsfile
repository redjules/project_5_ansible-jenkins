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
                sh "scp ${keyfile} root@64.226.65.202:/root/ssh-key.pem" 
            }
          }
        }
      }
    }
                  
  }
} 
