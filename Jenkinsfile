pipeline {
  agent any

  stages {

    stage('scm checkout') {
      steps {
        cleanWs()
        git branch: 'master', url: 'https://github.com/harshal2602/maven-project.git'
      }
    }

    stage('compile') {
      steps {
        withMaven(jdk: 'JDK_home', maven: 'MVN_HOME') {
          sh 'mvn compile'
        }
      }
    }

    stage('unit tests') {
      steps {
        withMaven(jdk: 'JDK_home', maven: 'MVN_HOME') {
          sh 'mvn test'
        }
      }
    }

    stage('package') {
      steps {
        withMaven(jdk: 'JDK_home', maven: 'MVN_HOME') {
          sh 'mvn clean package'
        }
      }
    }

    stage('create docker image') {
      steps {
        sh 'docker build --no-cache -t harshal2602/devops:latest .'
      }
    }

    stage('push image to dockerhub') {
      steps {
        withDockerRegistry(credentialsId: 'Docker_hub_credentials', url: 'https://index.docker.io/v1/') {
          sh 'docker push harshal2602/devops:latest'
        }
      }
    }

    stage('deploy container') {
      steps {
        sh '''
        docker stop devops-app || true
        docker rm devops-app || true
        docker pull harshal2602/devops:latest
        docker run -d -p 8080 --name devops-app harshal2602/devops:latest
        '''
      }
    }
  }
}
