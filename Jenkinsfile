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
        bat 'bat \'start /min stopApp.bat\''
        bat 'bat \'mvn install\''
        bat 'bat \'set JENKINS_NODE_COOKIE=dontKILLME && start /min startApp.bat\''
      }
    }

    stage('QA Deploy') {
      steps {
        echo 'QA Deploy'
        emailext(subject: 'QA Deploy', body: 'QA Deploy in process', to: 'gokul1027@gmail.com', from: 'gokul.jenkins@gmail.com')
      }
    }

    stage('Web UI') {
      parallel {
        stage('QA Build') {
          agent {
            node {
              customWorkspace 'workspace/webuiautomation'
              label 'API Test'
            }

          }
          steps {
            echo 'WEB UI'
            sleep(time: 10, unit: 'SECONDS')
            git(url: 'https://github.com/gokul1027/WebAppUiAutomation.git', branch: 'master')
          }
        }

        stage('API Test') {
          agent {
            node {
              customWorkspace 'workspace/webapiautomation'
              label 'API Test'
            }

          }
          steps {
            echo 'API Test'
            sleep(time: 10, unit: 'SECONDS')
            git(url: 'https://github.com/gokul1027/WebAppApiAutomation.git', branch: 'master')
            bat ' bat \'mvn test\''
          }
        }

      }
    }

    stage('QA Certify') {
      steps {
        echo 'QA Ceritified'
        emailext(subject: 'QA Certify', body: 'QA Certify', from: 'gokul.jenkins@gmail.com', replyTo: 'gokul1027@gmail.com', to: 'gokul1027@gmail.com')
      }
    }

    stage('Prod Deploy') {
      steps {
        echo 'PROD Deploy'
      }
    }

  }
}