pipeline{
    agent any
    tools {
      maven 'maven-3'
    }
    environment {
    DOCKER_TAG = "getVersion()"
    }
    stages{
        stage('SCM'){
            steps{
                git branch: 'main', url: 'https://github.com/demoshu23/docker-ansible'
            }
        }
        
        stage('Maven Build'){
            steps{
                sh "mvn clean package"
            }
        }
        stage('Docker Build'){
            steps{
                sh "docker build -t shu1demo/adhodoc:0.0.1 ."
            }
        }
        stage('tomcat deploy'){
            steps{
                sshagent(['tomcat-dev']) {
                   sh 'scp -o StrictKeyChecking=no target/*.war ec2-user@IP:/opt/tomcat8/webapps/'
                }
            }
        }
    }
}
    def getVersion(){
    def commitHash = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}

