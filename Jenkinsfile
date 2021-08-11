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
                        script {
                            if (sendmail == 'yes') {
                       emailext body: '''<body leftmargin="8" marginwidth="0" topmargin="8" marginheight="4"
                offset="0">
                <table width="95%" cellpadding="0" cellspacing="0"
                    style="font-size: 11pt; font-family: Tahoma, Arial, Helvetica, sans-serif">
                    <tr>
                        <td><br />
                        <b><font color="#0B610B">构建信息</font></b>
                        <hr size="2" width="100%" align="center" /></td>
                    </tr>
                    <tr>
                        <td>
                            <ul>
                                <li>构建名称：${JOB_NAME}</li>
                                <li>构建结果: <span style="color:green"> ${BUILD_STATUS}</span></li>
                                <li>构建编号：${BUILD_NUMBER}  </li>
                                <li>GIT 地址：${git_url}</li>
                                <li>GIT 分支：${git_branch}</li>
                                <li>变更记录: ${CHANGES,showPaths=true,showDependencies=true,format="<pre><ul><li>提交ID: %r</li><li>提交人：%a</li><li>提交时间：%d</li><li>提交信息：%m</li><li>提交文件：<br />%p</li></ul></pre>",pathFormat="         %p <br />"}
                            </ul>
                        </td>
                    </tr>
                </table>
            </body>
            </html>
            ''', subject: '${PROJECT_NAME}', to: 'wei.ma@jhlinux.com,'
                            }
                        }
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