node{
    echo "The Name is: ${env.JOB_NAME}" 
    echo "The Buid number is :  ${env.BUILD_NUMBER}"
    echo "The Node is name is:  ${env.NODE_NAME}"
    echo "The Jenkins HD is:  ${env.JENKINS_HOME}"
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    def mavenHome = tool name: 'maven3.8.7'
    stage('CheckOutCode'){
    git branch: 'development', credentialsId: '5cfbce6b-82ba-4399-9dc6-cd32b629ebec', url: 'https://github.com/omichael003/maven-web-application.git'    
    }
    
    stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage('ExcuteSonarQubeReport'){
        sh "${mavenHome}/bin/mvn clean sonar:sonar"
        
    }
    
    stage('UploadArtifactsIntoNexus'){
        sh "${mavenHome}/bin/mvn clean deploy"
    }
    
    stage('DeployAppInTomcatServer'){
        sshagent(['fcf1a702-3e62-4e96-8f27-feef0255ae0b']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.84.192:/opt/apache-tomcat-9.0.70/webapps/"
    }
    }
}
