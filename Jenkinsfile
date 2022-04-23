pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Build Demo Application'
        sh 'sh run_build_script.sh'
      }
    }

    stage('Linux Tests') {
      parallel {
        stage('Linux Tests') {
          steps {
            echo 'Run Linux tests'
            sh 'sh run_linux_tests.sh'
          }
        }

        stage('Windows Tests') {
          steps {
            echo 'Run Windows tests'
          }
        }

      }
    }

    stage('Deploy Staging') {
      steps {
        echo 'Deploy to Staging Environment'
        input 'Ok to deploy to product'
      }
    }

    stage('Deploy Production Stage') {
      post {
        always {
          archiveArtifacts(artifacts: 'target/demoapp.jar', fingerprint: true)
        }

        failure {
          emailext(subject: 'Demoapp build failure', to: 'ci-team@example.com', body: 'Build failure for demoapp Build ${env.JOB_NAME} ')
        }

      }
      steps {
        echo 'Deploy to Prod'
      }
    }

  }
}