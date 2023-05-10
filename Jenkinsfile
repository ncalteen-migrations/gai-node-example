pipeline {
  agent {
    docker {
      image 'node:18.16.0-alpine'
    }
  }

  // Environment variables
  environment {
    OWNER = 'ncalteen-migrations'
    REPO = 'gai-node-example'
  }

  stages {
    stage('Check Node') {
      steps {
        sh 'node --version'
      }
    }

    stage('Build') {
      steps {
        // Retry logic example
        retry(3) {
          sh 'npm ci'
        }
      }
    }
    
    stage('Test') {
      steps {
        sh 'npm test'
      }
      post {
        always {
          junit 'reports/**/*.xml'
        }
      }
    }

    stage('Publish') {
      steps {
        echo 'Publishing to @${OWNER}/${REPO}'
        sh 'npm publish'
      }
    }
  }
}