#!/usr/bin/env groovy

pipeline {
   agent any

    options {
        skipStagesAfterUnstable()
        timeout(time: 1, unit: 'HOURS')
        timestamps()
    }
    stages {
        stage('Init') {
            steps {
                echo 'test'
            }
        }
    }

    post {
        success {
          echo 'success'
        }

        always {
            deleteDir()
        }

        changed {
            echo "This prints only in case of changed to ${currentBuild.currentResult}"
            // Will trigger only when job status changes: GREEN -> RED, RED -> GREEN, etc
            // notifyStatusChange script: this, componentName: componentName
        }
    }
}
