pipeline{
    agent any
    tools {
      maven 'maven'
    }
    environment {
      DOCKER_TAG = getVersion()
    }
    stages{
        stage('SCM'){
            steps{
                git branch: 'pushpa', url: 'https://github.com/Pushpalathasaparapu/dockeransiblejenkins'
            }
        }
        
        stage('Maven Build'){
            steps{
                sh "mvn clean package"
            }
        }
        
        stage('Docker Build'){
            steps{
                sh "docker build . -t pushpasaparapu/ansible:${DOCKER_TAG} "
            }
        }
        
        stage('DockerHub Push'){
            steps{
                sh "docker login -u pushpasaparapu -p Pushpa@1234"   
                sh "docker push pushpasaparapu/ansible:${DOCKER_TAG} "
            }
        }
        
        stage('Docker Deploy'){
            steps{
              ansiblePlaybook credentialsId: 'tomcat', disableHostKeyChecking: true, extras: "-e DOCKER_TAG=${DOCKER_TAG}", installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
            }
        }
    }
}

def getVersion(){
    def commitHash = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
