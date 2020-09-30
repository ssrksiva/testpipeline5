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

    stage('Clean') {
      steps {
        sh 'chmod +x mvnw'
        sh './mvnw -ntp clean -P-webpack'
      }
    }

    stage('install tools') {
      steps {
        sh './mvnw -ntp com.github.eirslett:frontend-maven-plugin:install-node-and-npm -DnodeVersion=v10.16.0 -DnpmVersion=6.9.0'
      }
    }

    stage('npm install') {
      steps {
        sh './mvnw -ntp com.github.eirslett:frontend-maven-plugin:npm'
      }
    }

    stage('backend test') {
      steps {
        sh './mvnw -ntp verify -P-webpack'
      }
    }

    stage('front end') {
      steps {
        sh './mvnw -ntp com.github.eirslett:frontend-maven-plugin:npm -Dfrontend.npm.arguments=\'run test\''
      }
    }

    stage('packaging') {
      steps {
        sh './mvnw -ntp verify -P-webpack -Pprod -DskipTests'
        archiveArtifacts(fingerprint: true, artifacts: '**/target/*.jar')
      }
    }

    stage('Sonar') {
      steps {
        withSonarQubeEnv('My SonarQube Server') {
          sh './mvnw -ntp sonar:sonar'
        }

      }
    }

    stage('Deliver for development') {
      when {
        branch 'develop'
      }
      steps {
        sh 'git config --global user.name "ssrksiva"'
        sh 'git config --global user.email "sssrkbsc@gmail.com"'
        sh 'git tag -a tagName6 -m "test-admin6"'
        sh 'git commit -am "Merged develop branch to master"'
        sh 'git merge -s ours develop --allow-unrelated-histories'
        sh 'git config --global credential.username ssrksiva'
		sh 'git config --global credential.helper !echo password=14Dec@1991; echo'
		sh 'git push origin HEAD:master -f'
      }
    }

  }
}