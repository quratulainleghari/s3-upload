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
                git "https://github.com/john-smart/game-of-life.git"
               sh 'mvn clean package'
             // sh 'mvn package'
            }
          post {
                success {
                    archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                    junit '**/target/surefire-reports/*.xml' 
                   
s3Upload consoleLogLevel: 'INFO', dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 's3-bucket-jenkins', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-2', showDirectlyInBrowser: false, sourceFile: '**/target/surefire-reports/*.xml', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'arn:aws:s3:::s3-bucket-jenkins', userMetadata: []

                }
          
                
                
            }
         
         
      }
       
       
        stage('Deploy to Tomcat'){
  steps {
  sshagent(['3d0ff4fe-87e0-468b-9c6f-fbd6f291a57b']) {
    sh "scp  /var/lib/jenkins/workspace/maven-s3-pipeline/gameoflife-web/target/*.war ubuntu@18.234.76.16:/opt/tomcat/apache-tomcat-8.5.37/webapps"
    
    }
    }
    }
    }
}
