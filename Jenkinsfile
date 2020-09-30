pipeline {
  agent any
  stages {
    stage('Pull') {
      steps {
        git(url: 'https://github.com/ssrksiva/testpipeline5.git', branch: 'develop', poll: true)
      }
    }

    stage('Check Java') {
      steps {
        sh 'java -version'
      }
    }

    stage('Deliver for development') {
      when {
        branch 'develop'
      }
      steps {
        sh 'git config --global user.name "ssrksiva"'
        sh 'git config --global user.email "sssrkbsc@gmail.com"'
        withCredentials(bindings: [usernamePassword(credentialsId: 'testgithubcred', usernameVariable: 'user', passwordVariable: 'pass')]) {
          script {
            env.encodedPass=URLEncoder.encode(pass, "UTF-8")
          }

          sh 'git push https://${user}:${encodedPass}@github.com/${user}/testpipeline5.git HEAD:master -f --delete origin/develop'
        }

      }
    }

  }
}