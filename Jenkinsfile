pipeline {
    agent {
        docker {
            image 'node:latest' 
        }
    }
    stages {
        stage('NPM Clear cache') { 
            steps {
                echo 'Clear cache for npm'
                sh 'npm cache clean --force' 
            }
        }
        stage('Install all dependencies') { 
            steps {
                echo 'Install all dependencies which was declarate in package.json'
                sh 'npm install' 
            }
        }
        stage('Build our app') { 
            steps {
                echo 'Build our front'
                sh 'npm run build'
                sh 'tar czf build-${BUILD_NUMBER}.tar.gz build/'
            }
        }
         stage('SSH transfer') {
               steps {
                 sshPublisher(
                    continueOnError: false, failOnError: true,
                       publishers: [
                       sshPublisherDesc(
                        configName: "ec2-user@3.64.250.181",
                         verbose: true,
                           transfers: [
                             sshTransfer(
                             sourceFiles: "build/",
                             remoteDirectory: "/home/ec2-user/teachua/www/front/",
                             execCommand: "sudo docker restart apache_prod"
                )
          ])
      ])
   }
 }
    }
}
