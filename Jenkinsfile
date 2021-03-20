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
                sh 'tar czf build-${BUILD_NUMBER}.tar.gz ${SOURCE_FILES}'
            }
        }
        stage("Docker Login") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    
	         echo " ============== docker login =================="
                 sh 'docker login -u $USERNAME -p $PASSWORD'
                }
            }
        }
        stage('Build docker image for App') {
            steps{
                 echo " ============== docker build =================="
                 sh "docker build -t buchyk/front:${BUILD_NUMBER} ."
            }
        }
        stage("Docker Push") {
            steps {
                echo " ============== start pushing image =================="
                sh "docker push buchyk/front:${BUILD_NUMBER}"
            }
        }





    //      stage('SSH save to archive') {
    //            steps {
    //              sshPublisher(
    //                 continueOnError: false, failOnError: true,
    //                    publishers: [
    //                    sshPublisherDesc(
    //                        configName: "${DESTINATION_SERVER}",
    //                      verbose: true,
    //                        transfers: [
    //                          sshTransfer(
    //                          sourceFiles: "build-${BUILD_NUMBER}.tar.gz",
    //                              remoteDirectory: "${SAVE_ARCHIVE}"
    //                     )
    //                 ])
    //             ])
    //          }
    //   }
    //      stage('SSH transfer') {
    //            steps {
    //              sshPublisher(
    //                 continueOnError: false, failOnError: true,
    //                    publishers: [
    //                    sshPublisherDesc(
    //                        configName: "${DESTINATION_SERVER}",
    //                      verbose: true,
    //                        transfers: [
    //                          sshTransfer(
    //                              sourceFiles: "${SOURCE_FILES}",
    //                              remoteDirectory: "${REMOTE_WORK_DIR}",
    //                 )
    //             ])
    //         ])
    //      }
    //   }
    }
}
