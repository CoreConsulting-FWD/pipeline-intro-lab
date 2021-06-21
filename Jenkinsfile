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
      parallel {
        stage('Backend') {
          steps {
            sh 'chmod u+x ./jenkins/test-backend.sh'
            sh './jenkins/test-backend.sh'
            junit 'target/surefire-reports/**/TEST*.xml'
          }
        }

        stage('Frontend') {
          steps {
            sh 'chmod u+x ./jenkins/test-frontend.sh'
            sh './jenkins/test-frontend.sh'
            junit 'target/test-results/**/TEST*.xml'
          }
        }

        stage('Performance') {
          steps {
            sh 'chmod u+x ./jenkins/test-performance.sh'
            sh './jenkins/test-performance.sh'
          }
        }

        stage('Static') {
          steps {
            sh 'chmod u+x ./jenkins/test-static.sh'
            sh './jenkins/test-static.sh'
          }
        }

      }
    }

    stage('Fluffy Deploy') {
      steps {
        input(message: 'Confirm Deploy', ok: 'Yes')
        sh './jenkins/deploy.sh staging'
      }
    }

  }
  environment {
    PATH = "/opt/apache-maven-3.5.4/bin:$PATH"
  }
}