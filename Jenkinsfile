pipeline {
  agent {
    label 'vagrant'
  }
  stages {
    stage('Build') {
      steps {
        git 'https://github.com/rguruju/jenkins-demo.git'
        wrap([$class: 'BuildUser']) {
          sh './gradlew clean build uploadArchives -PbuildNumber=${BUILD_NUMBER} -Pusername=${BUILD_USER_ID}'
        }
        publishHTML(
          [allowMissing: true,
          alwaysLinkToLastBuild: true,
          keepAll: true,
          reportDir: 'build/reports/tests/test',
          reportFiles: 'index.html',
          reportName: 'UnitTestReport',
          reportTitles: ''])
      }
    }

    stage('Deploy') {
      steps {
        sh 'ssh vagrant@172.28.128.3 "pkill -9 -f spring-music && rm -rf *.jar"'
        sh 'scp ./build/libs/hello-service-1.0.${BUILD_NUMBER}.jar vagrant@172.28.128.3:/home/vagrant'
        sh 'ssh vagrant@172.28.128.3 "nohup java -jar /home/vagrant/hello-service-1.0.${BUILD_NUMBER}.jar &"'
      }
    }
  }
}
