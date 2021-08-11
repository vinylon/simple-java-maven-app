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
    stage('Test') {
        steps {
            sh 'mvn test'
        }
        post {
            always {
                junit 'target/surefire-reports/*.xml'
            }

            success {
               emailext (
                    subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                    body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                        <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
                    to: "wei.ma@jhlinux.com",
                    from: "wei.ma@jhlinux.com"
               )
            }
            failure {
                emailext (
                    subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                    body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                        <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
                    to: "wei.ma@jhlinux.com",
                    from: "wei.ma@jhlinux.com"
                )
            }
        }

    }
    stage('Deliver') {
        steps {
            sh './jenkins/scripts/deliver.sh'
        }
    }
  }
}