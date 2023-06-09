pipeline {
  agent {
    docker {
      image 'node:18.16.0-alpine'
      args '-u root:root'
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

    stage('Generate Cucumber Reports') {
      steps {
        sh 'npx cucumber-js --config ./cucumber.json'
        cucumber buildStatus: "UNSTABLE",
                 fileIncludePattern: "**/cucumber-report.json"
      }
    }

    stage('Publish') {
      steps {
        echo 'Publishing to @${OWNER}/${REPO}'
        sh 'npm publish --registry https://npm.pkg.github.com/'
      }
    }
  }
}