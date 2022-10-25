pipeline{
    agent any
    tools {
      maven 'maven3'
    }
    parameters {
      choice choices: ['Main', 'Feature-1', 'Gokul', 'master'], description: 'Choose the branch', name: 'Branch'
    }
    stages{
        stage("Git checkout"){
            steps{
                git branch: Branch ,url: 'https://github.com/gokulgk7989/New-webapp.git' 
            }
        }
        stage("Maven build"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("Deployment to Tomcat"){
            steps{
                sshagent(['Tomcat']){
                    sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.42.56:/opt/tomcat9/webapps"
                    sh "ssh ec2-user@172.31.42.56 /opt/tomcat9/bin/shutdown.sh"
                    sh "ssh ec2-user@172.31.42.56 /opt/tomcat9/bin/startup.sh"
                }
            }
        }
    }
    post {
          success {
            archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
          }
        }
}
