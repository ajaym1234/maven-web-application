node { 
 // global variables used
 echo " the node name is : ${env.NODE_Name} "
 echo " the build number is :${env.BUILD_NUMBER} "
  
 buildName 'dev-#${BUILD_NUMBER}'
 properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: '']])
 
 def mavenHome= tool name :"maven3.9.3"
    
    stage ('checkout'){
        
    git credentialsId: 'ea096c9c-704c-457b-aa2d-2820ba126d0d', url: 'https://github.com/ajaym1234/maven-web-application.git'
    
}// Build stage
stage('Build'){
sh "$mavenHome/bin/mvn clean package"
 }
// generate sonarqube report
stage('sonarqube report'){
sh "$mavenHome/bin/mvn sonar:sonar"
 } 
 stage('deployartifactTonexus'){
      sh "$mavenHome/bin/mvn deploy"
 }
 stage('tomcat dploy'){
 sshagent(['77be5c90-cc68-433b-a791-af6229bdd5e9']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.20.248:/opt/tomcat9/webapps"
}
}

    
}
