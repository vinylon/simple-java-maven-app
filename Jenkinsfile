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
        sh 'mvn -B -DskipTests clean package'
      }
      post {
        success {
            echo "构建成功"
        }
        failure {
            echo "构建失败"
        }
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
               echo "单元测试成功"
               sh 'ls target/surefire-reports'
               emailext (
                    attachmentsPattern: "target/surefire-reports/*.txt",
                    subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                    body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                        <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
                    to: "wei.ma@jhlinux.com",
                    from: "wei.ma@jhlinux.com"
               )
            }
            failure {
                echo "单元测试失败"
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
    stage('JIRA') {
        withEnv(['JIRA_SITE=my-jira']) {
             def testIssue = [fields: [ project: [key: 'apm'], summary: 'New JIRA Created from Jenkins.',description: 'New JIRA Created from Jenkins.',issuetype: [name: '故事']]]

             response = jiraNewIssue issue: testIssue

             echo response.successful.toString()
             echo response.data.toString()
       }
    }
    stage('Deliver') {
        steps {
            sh './jenkins/scripts/deliver.sh'
        }
    }
  }
}