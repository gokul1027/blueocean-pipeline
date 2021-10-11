pipeline {
  agent any
  stages {
    stage('Dev Deploy') {
      steps {
        echo 'Dev Deploy'
        git(url: 'https://github.com/gokul1027/WebApp.git', branch: 'master')
      }
    }

    stage('Dev Build') {
      steps {
        echo 'Dev Build'
        bat 'start /min stopApp.bat'
        bat 'mvn install'
        bat 'set JENKINS_NODE_COOKIE=dontKILLME && start /min startApp.bat'
      }
    }

    stage('QA Deploy') {
      steps {
        echo 'QA Deloyed'
        emailext(subject: 'QA Deployment', body: 'QA Deployment in progress', replyTo: 'gokul.jenkins@gmail.com', to: 'gokul1027@gmail.com', from: 'gokul.jenkins@gmail.com')
      }
    }

    stage('QA Build') {
      parallel {
        stage('QA UI Build') {
          agent any
          steps {
            echo 'Web Test'
            git(url: 'https://github.com/gokul1027/WebAppUiAutomation.git', branch: 'master')
            sleep 10
            bat 'mvn test'
          }
        }

        stage('QA API Build') {
          agent any
          steps {
            echo 'API Test'
            git(url: 'https://github.com/gokul1027/WebAppApiAutomation.git', branch: 'master')
            sleep 10
            bat 'mvn test'
          }
        }

      }
    }

    stage('QA Certify') {
      steps {
        echo 'QA certify'
        input(message: 'Do you like to certify?', id: 'Certify', ok: 'Yes')
      }
    }

    stage('Prod deployed') {
      steps {
        echo 'Prod Deployed'
        emailext(subject: 'Prod Deployed', body: 'Prod Deployed Success', from: 'gokul.jenkins@gmail.com', replyTo: 'gokul1027@gmail.com', to: 'gokul1027@gmail,com')
      }
    }

  }
}