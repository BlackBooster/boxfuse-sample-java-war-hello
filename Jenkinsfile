@Library('common-functions@master')
import common.function.CommonFunction

// def funct = new CommonFunction(this)

pipeline {
  agent any
  tools {
    maven 'maven-3.6.1'
  }
  stages {
    stage('Unit Tests') {
      steps {
        sh 'mvn surefire:test'
      }
    }
    stage('Integration Tests') {
      steps {
        sh 'mvn failsafe:integration-test'
      }
    }
    stage('Version Set') {
      steps {
        sh "mvn versions:set -DnewVersion=1.0.${BUILD_NUMBER}"
      }
    }
    stage('Upload to Nexus') {
      steps {
        script {
          def funct = new CommonFunction(this)
          funct.mvnPlugIn('deploy -DskipTests=true')
        }
      }
    }
    stage('Clean workspace') {
      steps {
        cleanWs()
      }
    }
  }
}
