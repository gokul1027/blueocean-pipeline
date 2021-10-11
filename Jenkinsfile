pipeline {
  agent any
  stages {
    stage('Dev Code Pull') {
      steps {
        echo 'Dev Code Pull'
        git(url: 'https://github.com/gokul1027/WebApp.git', branch: 'master')
      }
    }

    stage('Dev Code Build') {
      steps {
        echo 'Dev Code Build'
        bat 'start /min stopApp.bat'
        bat 'mvn install'
        bat 'set JENKINS_NODE_COOKIE=dontKILLME && start /min startApp.bat'
      }
    }

    stage('QA Deploy') {
      steps {
        echo 'QA Deploy'
      }
    }

    stage('UI Test') {
      parallel {
        stage('UI Test') {
          steps {
            echo 'UI Test'
            git(url: 'https://github.com/gokul1027/WebAppUiAutomation.git', branch: 'master')
            sleep(time: 10, unit: 'SECONDS')
            bat 'mvn test'
          }
        }

        stage('API test') {
          steps {
            echo 'API Test'
            git(url: 'https://github.com/gokul1027/WebAppApiAutomation.git', branch: 'master')
            sleep(time: 10, unit: 'SECONDS')
            bat 'mvn test'
          }
        }

      }
    }

    stage('QA Certify') {
      steps {
        echo 'QA Certified'
        input(message: 'Do you like to certify?', id: 'Certify', ok: 'Yes')
      }
    }

    stage('UAT Deploy') {
      steps {
        echo 'UAT Deploy'
      }
    }

    stage('UAT Certify') {
      steps {
        echo 'UAT Certify'
        input(message: 'Do you like to certify?', id: 'Certify', ok: 'Yes')
      }
    }

    stage('Prod Deploy') {
      steps {
        echo 'Prod Deploy'
      }
    }

  }
}