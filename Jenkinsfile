pipeline {
  agent {
    docker {
      image 'node:6-alpine'
      args '-p 3000:3000'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test') {
      environment {
        CI = 'true'
      }
      steps {
        sh './jenkins/scripts/test.sh'
      }
    }
    stage('Deliver') {
      steps {
        sh './jenkins/scripts/deliver.sh'
        input 'Finished using the web site? (Click "Proceed" to continue)'
        sh './jenkins/scripts/kill.sh'
      }
    }
    stage('OverOps') {
      steps {
        OverOpsQuery(applicationName: '${JOB_NAME}', deploymentName: '${BUILD_NUMBER}', activeTimespan: 60, baselineTimespan: 10080, criticalExceptionTypes: 'NullPointerException,IndexOutOfBoundsException,InvalidCastException,AssertionError', minVolumeThreshold: 1, minErrorRateThreshold: 1, regressionDelta: 0.5, criticalRegressionDelta: 1, applySeasonality: true, markUnstable: true, showResults: true, printTopIssues: 10, maxErrorVolume: 1, maxUniqueErrors: 1, regexFilter: '"type":\\"*(Timer|Logged Warning)', verbose: true, serverWait: 10, serviceId: 'S37295')
      }
    }
  }
}