pipeline {
  agent any
  stages {
    stage('Server') {
      agent {
        docker {
          image '3.6-jdk-8-slim'
        }

      }
      steps {
        sh '''echo "Hello World From Blue Ocean"
echo "Building Server"
mvn -version

mkdir -p target
touch "target/server.war"'''
      }
    }

  }
}