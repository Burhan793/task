
// Jenkinsfile (place at repository root)
pipeline {
  agent any
  stages {
    stage('Checkout Code') {
      steps {
        git branch: 'main', url: 'https://github.com/Burhan793/task.git'
      }
    }
    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }
    stage('Run Tests') {
      steps {
        bat 'npm test'
      }
    }
  }
  post {
    success {
      echo 'Build and tests successful!'
    }
    failure {
      echo 'Build failed or tests failed!'
    }
  }
}
