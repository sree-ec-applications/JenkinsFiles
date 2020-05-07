node
{
def mavenHome= tool name: 'maven3.6.2'
   //echo "GitHub BranhName ${env.BRANCH_NAME}"
  echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
   
  properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '2', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
   
stage('checkoutCode')
{
git branch: 'development', credentialsId: '388d441c-4558-4bb6-8231-9dc7340117ee', url: 'https://github.com/sree-ec-applications/maven-web-application.git '
}  
   
stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}

stage('SonarQubeReportExecution')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtfactsIntoNexus')
{
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcat')
{
sshagent(['0c54ecdb-6a2a-4be0-94d7-c7f1bcdaf0b6']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.54.12:/opt/apache-tomcat-9.0.34/webapps/"
}

}

stage('EmailNotification')
{
emailext body: '''Build is over

Regards,
Sreelakshmi''', subject: 'Build is over', to: 'sreelakshmireddivari@gmail.com'
}

}
