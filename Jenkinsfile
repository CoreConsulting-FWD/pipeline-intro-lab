pipeline {
  agent any
  stages {
    stage('Fluffy Build') {
      steps {
        sh 'chmod u+x ./jenkins/build.sh'
        sh './jenkins/build.sh'
        archiveArtifacts(artifacts: 'target/*.jar', fingerprint: true)
      }
    }

    stage('Fluffy Test') {
      steps {
        sh 'chmod u+x ./jenkins/test-all.sh ./jenkins/test-backend.sh ./jenkins/test-frontend.sh ./jenkins/test-static.sh ./jenkins/test-performance.sh'
        sh './jenkins/test-all.sh'
        junit '****/surefire-reports/**/*.xml'
      }
    }

    stage('Fluffy Deploy') {
      steps {
        sh './jenkins/deploy.sh staging'
      }
    }

  }
  environment {
    PATH = "/opt/apache-maven-3.5.4/bin:$PATH"
  }
}