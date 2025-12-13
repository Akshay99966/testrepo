pipeline {
  agent any

  environment {
    SNYK_TOKEN = credentials('snyk-token')
  }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Snyk Open Source') {
      when {
        anyOf {
          branch 'main'
          branch 'master'
          changeRequest()
        }
      }
      steps {
        sh '''
          npm install -g snyk
          snyk auth $SNYK_TOKEN
          snyk test
        '''
      }
    }

    stage('Snyk Code') {
      when {
        anyOf {
          branch 'main'
          branch 'master'
          changeRequest()
        }
      }
      steps {
        sh '''
          npm install -g snyk
          snyk auth $SNYK_TOKEN
          snyk code test
        '''
      }
    }
  }

  post {
    success {
      echo '✅ Snyk Open Source and Snyk Code checks passed'
    }
    failure {
      echo '❌ Snyk security checks failed'
    }
  }
}
