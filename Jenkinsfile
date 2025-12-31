pipeline {
  agent any
  stages {
    stage('scm checkout') {
      steps {
        git branch: 'master', url: 'https://github.com/harshal2602/maven-project.git'
      }
    }

    stage('compile the job') //validate then compile
    {
      steps {
        withMaven(globalMavenSettingsConfig: '', jdk: 'JDK_home', maven: 'MVN_HOME', mavenSettingsConfig: '', traceability: true) {
          sh 'mvn compile'
        }
      }
    }

    stage('execute unit test framework') {
      steps {
        withMaven(globalMavenSettingsConfig: '', jdk: 'JDK_home', maven: 'MVN_HOME', mavenSettingsConfig: '', traceability: true) {
          sh 'mvn test'
        }
      }
    }
    stage('build the code') {
      steps {
        withMaven(globalMavenSettingsConfig: '', jdk: '', maven: 'MVN_HOME', mavenSettingsConfig: '', traceability: true) {
          sh 'mvn clean package'
        }
      }
    }
    stage('create docker image') {
      steps {
        sh 'docker build -t harshal2602/devops:latest .'
      }
    }
    stage('push docker image to dockerhub') {
      steps {
        
        withDockerRegistry(credentialsId: 'DockerHubCredentials', url: 'https://index.docker.io/v1/') {
            
                sh 'docker push harshal2602/devops:latest'
            
        }
      }
    }
  }
}
