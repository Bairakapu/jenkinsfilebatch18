node
{

   echo "GitHub BranhName ${env.BRANCH_NAME}"
      echo "Jenkins Job Number ${env.BUILD_NUMBER}"
      echo "Jenkins Node Name ${env.NODE_NAME}"
  
      echo "Jenkins Home ${env.JENKINS_HOME}"
      echo "Jenkins URL ${env.JENKINS_URL}"
      echo "JOB Name ${env.JOB_NAME}"
  
properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])

def mavenHome = tool name: "maven3.6.2"


stage('CheckoutCode')
{
  git branch: 'development', credentialsId: '637894c5-5fe6-4f99-8051-0cb517ca4c6d', url: 'https://github.com/Bairakapu/maven-web-application.git'
}

stage('Build')
{
 
 sh "${mavenHome}/bin/mvn clean package"
}

stage('SonarQubeReportExecution')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactIntoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppintoTomcatServer')
{
 sshagent(['dfd1d184-197a-4374-8c77-586bf3c3296c']) 
 {
   sh "scp -o StrictHostKeyChecking=no  target/maven-web-application.war ec2-user@35.154.135.188:/opt/apache-tomcat-9.0.36/webapps/"
 }
}
  
}
