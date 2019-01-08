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
                   
                   withAWS(region:'us-east-2',credentials:'c165185d-97cb-4139-a553-df4833faecc3') {

                 def identity=awsIdentity();//Log AWS credentials
              s3Upload(bucket:"s3-bucket-jenkins", includePathPattern:'**/*');
                  // withAWS(credentials: '5f00510d-4d3b-4ac7-a914-225bb76a29fe', profile: '6502-0956-5639', region: 'us-east-2') {
//s3CopyArtifact buildSelector: workspace(), excludeFilter: '', filter: '**', flatten: false, optional: true, projectName: 'maven-s3-pipeline', target: ''

}

//s3Upload consoleLogLevel: 'INFO', dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 's3-bucket-jenkins', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-2', showDirectlyInBrowser: false, sourceFile: '**/target/surefire-reports/*.xml', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'arn:aws:s3:::s3-bucket-jenkins', userMetadata: []

                }
          
                
                
            }
         
         
      }
   
   //stage ('upload'){
     // steps {
     // withAWS(region:'us-east-2',credentials:'c165185d-97cb-4139-a553-df4833faecc3') {

                 //def identity=awsIdentity();//Log AWS credentials
//
                // Upload files from working directory 'dist' in your project workspace
              //  s3Upload(bucket:"s3-bucket-jenkins", includePathPattern:'**/*');
          //  }
  // }
  // }
   
       
       
        stage('Deploy to Tomcat'){
  steps {
  sshagent(['3d0ff4fe-87e0-468b-9c6f-fbd6f291a57b']) {
    sh "scp  /var/lib/jenkins/workspace/maven-s3-pipeline/gameoflife-web/target/*.war ubuntu@18.204.34.144:/opt/tomcat/apache-tomcat-8.5.37/webapps"
    
    }
    }
    }
    }

}
