node
{
    def mavenHome = tool name: "maven3.6.2"
    stage("get the code")
    {
        git branch: 'development', credentialsId: '572ca6cc-bbb0-430a-a91b-f9f3e47f62a6', url: 'https://github.com/dreamz25/maven-web-application.git'
    }
    stage('build')
    {
    sh "${mavenHome}/bin/mvn clean package"
    }
    stage("generate sonarqube report")
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage("Upload artifact into nexus repository")
    {
    sh "${mavenHome}/bin/mvn clean deploy"
    }
    stage("deployapplicationintoTomcatserver")
    {
        sshagent(['78da0d8e-9e81-4e53-81a9-e4a37000ad13'])
        {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.245.23:/opt/apache-tomcat-9.0.39/webapps/"

        }
        stage("sendingEmailNotification")
        {
            mail bcc: '', body: '''build over.......

Regards
Suresh kumar''', cc: 'ksureshkumar2510@gmail.com', from: '', replyTo: '', subject: 'build over', to: 'ksureshkumar1025@gmail.com'
        }
            }
}
