pipeline {
  agent any

  stages {

    stage('SCM Checkout') {
      steps {
        cleanWs()
        git branch: 'master', url: 'https://github.com/harshal2602/maven-project.git'
      }
    }

    stage('Build WAR') {
      steps {
        withMaven(jdk: 'JDK_home', maven: 'MVN_HOME') {
          sh 'mvn clean package'
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build --no-cache -t harshal2602/devops:latest .'
      }
    }

    stage('Push Image to DockerHub') {
      steps {
        withDockerRegistry(credentialsId: 'Docker_hub_credentials', url: 'https://index.docker.io/v1/') {
          sh 'docker push harshal2602/devops:latest'
        }
      }
    }

    stage('Deploy Container') {
      steps {
        sh '''
        docker stop cont3 || true
        docker rm cont3 || true
        docker pull harshal2602/devops:latest
        docker run -d -p 80:8080 --name cont3 harshal2602/devops:latest
        '''
      }
    }
  }
}
