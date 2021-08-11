pipeline {
  agent {
    docker {
      args '-v /root/.m2:/root/.m2'
      image 'maven:3.8-openjdk-8'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh '''mvn -v
mvn -B -DskipTests clean package'''
      }
    }

  }
}