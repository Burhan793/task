pipeline {
  agent any

  environment {
    TZ = 'Asia/Karachi'
  }

  stages {
    stage('Prepare') {
      steps {
        script {
          def buildNum = env.BUILD_NUMBER
          def ts = new Date().format("yyyy-MM-dd HH:mm:ss", TimeZone.getTimeZone(env.TZ))
          echo "Build #${buildNum} started at ${ts}"

          echo "Build causes (raw):"
          try {
            def causes = currentBuild.rawBuild.getCauses()
            for (c in causes) {
              echo " - ${c}"
            }
          } catch (err) {
            echo "Could not read raw build causes: ${err}"
          }

          boolean triggeredByGitHub = false
          try {
            def causes = currentBuild.rawBuild.getCauses()
            for (c in causes) {
              def cname = c.getClass().getName().toLowerCase()
              def cstr = c.toString().toLowerCase()
              if (cname.contains("remote") || cname.contains("github") || cname.contains("scmtrigger") || cstr.contains("github") || cstr.contains("hook") || cstr.contains("push") || cstr.contains("githubwebhook")) {
                triggeredByGitHub = true
              }
            }
          } catch (err) {
            if (env.GITHUB_EVENT_NAME) {
              triggeredByGitHub = true
            }
          }

          if (triggeredByGitHub) {
            echo "Triggered by GitHub Webhook"
          } else {
            echo "Not triggered by GitHub Webhook (manual or other trigger)"
          }

          currentBuild.description = "TriggeredByGitHub=${triggeredByGitHub}"
          env.TRIGGERED_BY_GH = triggeredByGitHub.toString()
        }
      }
    }

    stage('Checkout Code') {
      steps {
        git branch: 'main', url: 'https://github.com/Burhan793/task.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        echo 'Running npm install...'
        bat 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        echo 'Running npm test...'
        bat 'npm test'
      }
    }
  }

  post {
    success {
      echo "Build #${env.BUILD_NUMBER} succeeded at ${new Date().format('yyyy-MM-dd HH:mm:ss', TimeZone.getTimeZone('Asia/Karachi'))}"
    }
    failure {
      echo "Build #${env.BUILD_NUMBER} failed at ${new Date().format('yyyy-MM-dd HH:mm:ss', TimeZone.getTimeZone('Asia/Karachi'))}"
    }
  }
}

