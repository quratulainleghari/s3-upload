pipeline {
   agent {label "master"}
    
    tools {
        maven 'maven'
        jdk '1.8'
       
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

       
      
      stage ('Compile-Package') {
            steps {
                git "https://github.com/quratulainleghari/my-app.git"
               sh 'mvn -f /var/lib/jenkins/workspace/sonar-pipeline/my-app-master package'
             // sh 'mvn package'
            }
          post {
                success {
                    s3Upload consoleLogLevel: 'INFO', dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: '', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: '**/target/surefire-reports/*.xml', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'arn:aws:s3:::jenkins-bucket-qurat', userMetadata: []

}
            }
      }
