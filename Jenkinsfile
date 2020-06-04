pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Server') {
          agent {
            docker {
              image 'maven:3.6-jdk-8-slim'
              args '-u 0:0'
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
chmod -R 777 dist
cat > dist/index.html <<EOF
<HTML>
<Title> THIS IS TESTING BLUEOCEAN PAGE </TITLE>
<h1> HELLO Works Like Charm </h1>
EOF
touch "dist/client.js"'''
            stash(name: 'client', includes: '**/dist/*')
          }
        }

      }
    }

    stage('Testing') {
      parallel {
        stage('Chrome') {
          agent {
            docker {
              image 'selenium/standalone-chrome'
            }

          }
          steps {
            sh '''echo "Hello From Chrome Testing Stage"
echo \'mvn test -Dbrowser=chrome\''''
          }
        }

        stage('Firefox') {
          agent {
            docker {
              image 'selenium/standalone-firefox'
            }

          }
          steps {
            sh 'echo "This is Server Testing"'
            sh '''echo "Hello From Firefox Stage"
echo \'mvn test -Dbrowser=firefox\''''
          }
        }

      }
    }

  }
}