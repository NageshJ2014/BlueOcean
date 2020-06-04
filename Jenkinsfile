pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Server') {
          agent {
            docker {
              image 'maven:3.6-jdk-8-slim'
            }

          }
          steps {
            sh '''echo "Hello World From Blue Ocean"
echo "Building Server"
mvn -version

mkdir -p target
touch "target/server.war"'''
            stash(name: 'server', includes: '**/*.war')
          }
        }

        stage('Client') {
          agent {
            docker {
              image 'node:13'
              args '-u 0:0'
            }

          }
          steps {
            sh '''echo "Building the Client Code"
npm install --save react
mkdir -p dist
cat > dist/index.html <<EOF
<HTML>
<Title> THIS IS TESTING BLUEOCEAN PAGE </TITLE>
<h1> HELLO Works Like Charm </h1>
EOF
touch "dist/client.js"'''
          }
        }

      }
    }

    stage('Testing') {
      parallel {
        stage('ServerTest') {
          steps {
            sh 'echo "Hello World"'
          }
        }

        stage('ClientTest') {
          steps {
            sh 'echo "This is Server Testing"'
          }
        }

      }
    }

  }
}