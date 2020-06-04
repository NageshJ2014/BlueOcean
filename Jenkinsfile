pipeline {
  agent any
  stages {
    stage('Build Test') {
      parallel {
        stage('Build Test') {
          steps {
            sh 'echo "Hello World From Blue Ocean"'
          }
        }

        stage('Parallel Stage') {
          steps {
            echo 'PARALLEL BUILD MESSAGE'
          }
        }

      }
    }

    stage('Second Stage') {
      steps {
        input(message: 'BUILD Waiting', id: 'TEST', ok: 'Wow Works')
      }
    }

  }
}