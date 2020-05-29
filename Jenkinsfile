pipeline {
     agent any
     stages {
         stage("Build") {
             steps {
                 sh 'echo "Hello World"'
                 sh '''
                     echo "Multiline shell steps works too"
                     ls -lah
                 '''
             }
         }
         stage("Lint HTML") {
            steps{
                sh 'tidy -q -e *.html'
                }
        }
         stage('Security Scan') {
              steps { 
                 aquaMicroscanner imageName: 'alpine:latest', notCompleted: 'exit 1', onDisallowed: 'fail'
              }
         }         
         stage("Upload to AWS") {
            steps{
                withAWS(credentials: 'blueocean', region: 'us-east-2'){
                    s3Upload bucket: 'shree-static-jenkins-pipeline', file: 'index.html', pathStyleAccessEnabled: true, payloadSigningEnabled: true
                }          
            }
        }
     }
}
