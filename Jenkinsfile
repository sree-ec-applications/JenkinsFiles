node
{
def mavenHome= tool name: 'maven3.6.2'
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
