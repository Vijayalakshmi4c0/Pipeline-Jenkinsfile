
node
{
def mavenHome= tool name: 'maven3.6.2'

stage('CheckoutCode')
{
git branch: 'development', credentialsId: '3281f36d-7687-4c0c-943c-8b53bbecf4ea', url: 'https://github.com/Vijayalakshmi4c0/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}

stage('SonarQubeReortExecution')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactsIntoNexus')
{
sh "${mavenHome}/bin/mvn clean deploy"
}

stage ('DeployAppIntoTomcat')
{
sshagent(['4bba0b38-341e-4a51-b6c9-451be5dd3a84']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.137.145:/opt/apache-tomcat-9.0.34/webapps/"    
}
}

stage('Email Notification')
{
emailext body: '''Build is over.

Regards,
Vijayalakshmi.''', subject: 'Build is over', to: 'vijjiabi@gmail.com'
}

}
