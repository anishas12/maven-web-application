node('node-wallmart'){
    def mavenHome = tool name: "maven3.8.4"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    stage('CheckoutCode'){
        git branch: 'development', credentialsId: 'd6adfa7c-2116-4816-8e6b-a2b14efc4503', url: 'https://github.com/anishas12/maven-web-application.git'
    }
    stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('ExecuteSonarQubeReport'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('UploadArtifactsIntoNexus'){
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('DeployAppIntoTomcatServer'){
    sshagent(['e9fa719b-3697-4af5-a5d9-8f1c52e3324e']){
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.144.139.8:/opt/apache-tomcat-9.0.59/webapps"
    }
    }
    stage('SendEmailNotification'){
        emailext body: '''build over


regards 
anu''', subject: 'build over', to: 'anishasalvaji@gmail.com'
    }
    
}
