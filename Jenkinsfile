pipeline{
    agent any
    tools {
      maven 'maven3'
    }
    options {
      buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '3', numToKeepStr: '3')
    }
    parameters {
      choice choices: ['master', 'Feature-1', 'Feature-2', 'Gokul', 'development', 'production', 'Raju'], description: 'Select the branch', name: 'Branch'
    }

    stages{
        stage("Git Checkout"){
            steps{
                git branch: Branch, url: 'https://github.com/gokulgk7989/New-webapp'
            }
        }
        stage("maven build"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("Tomcat Deployment"){
            steps{
                sshagent(['tomcat-dev']){
                    sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.44.98:/opt/tomcat9/webapps"
                    sh "ssh ec2-user@172.31.44.98 sudo /opt/tomcat9/bin/shutdown.sh"
                    sh "ssh ec2-user@172.31.44.98 sudo /opt/tomcat9/bin/startup.sh"
                }
            }
        }
    }
    post {
  success {
    archiveArtifacts artifacts: 'target/*war', followSymlinks: false
  }
}

}
