pipeline {
  agent any
  stages {
    stage('Dev Build') {
      steps {
        echo 'Dev Build'
        git(url: 'https://github.com/gokul1027/WebApp.git', branch: 'master')
      }
    }

    stage('Dev Deployed') {
      steps {
        echo 'Dev Deployed'
        bat 'start /min stopApp.bat'
        bat 'mvn install'
        bat 'set JENKINS_NODE_COOKIE=dontKILLME && start /min startApp.bat'
      }
    }

    stage('QA Test') {
      parallel {
        stage('Web Test') {
          agent {
            node {
              customWorkspace 'workspace/webuiautomation'
              label 'web test'
            }

          }
          steps {
            echo 'Web Test'
            git(url: 'https://github.com/gokul1027/WebAppUiAutomation.git', branch: 'master')
            sleep 10
            bat 'mvn test'
          }
        }

        stage('API Test') {
          agent {
            node {
              label 'hpslave'
              customWorkspace 'C:/Users/gokul/Desktop/ubuntu/jenkins/webuiautomation'
            }

          }
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
        echo 'QA Certify'
        input(message: 'Do you like to certify?', id: 'Certify', ok: 'Yes')
      }
    }

    stage('Prod Deployed') {
      steps {
        echo 'Prod Deploy'
      }
    }

  }
}