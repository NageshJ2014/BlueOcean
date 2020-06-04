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
chmod -R 777 target
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

    stage('QA Testing') {
      agent {
        docker {
          image 'tomcat:10-jdk11'
          args '-u 0:0 -p 11080:8080'
        }

      }
      steps {
        unstash 'server'
        unstash 'client'
        sh '''APP_DIR=/usr/local/tomcat/webapps
rm -rf $APP_DIR/ROOT
cp target/server.war $APP_DIR/server.war
mkdir -p $APP_DIR/ROOT
cp dist/* $APP_DIR/ROOT/
/usr/local/tomcat/bin/startup.sh'''
        input(message: 'Waiting For QA Confirmation', id: 'QATEST', ok: 'Wow Works', submitter: 'Nagesh')
      }
    }

  }
}