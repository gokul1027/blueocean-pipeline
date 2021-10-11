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
        bat 'bat \'start /min stopApp.bat\'             bat \'mvn install\'             bat \'set JENKINS_NODE_COOKIE=dontKILLME && start /min startApp.bat\''
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
            sleep 10
            bat 'bat \'mvn test\''
          }
        }

        stage('API test') {
          steps {
            echo 'API Test'
            git(url: 'https://github.com/gokul1027/WebAppApiAutomation.git', branch: 'master')
            sleep 10
            bat 'bat \'mvn test\''
          }
        }

      }
    }

    stage('QA Certify') {
      steps {
        echo 'QA Certified'
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
      }
    }

    stage('Prod Deploy') {
      steps {
        echo 'Prod Deploy'
      }
    }

  }
}