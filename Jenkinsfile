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
               setBuildStatus("In progress...", "PENDING");
                echo 'test'
            }
        }
       stage('Test') {
            steps {
                echo 'test'
               sh 'pwd'
               sh 'whoami'
            }
        }
    }

    post {
        success {
          echo 'success'
        }

        always {
           setBuildStatus(${currentBuild.currentResult}, ${currentBuild.currentResult});
            deleteDir()
        }

        changed {
            echo "This prints only in case of changed to ${currentBuild.currentResult}"
            // Will trigger only when job status changes: GREEN -> RED, RED -> GREEN, etc
            // notifyStatusChange script: this, componentName: componentName
        }
    }
}
void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/Kyroy/jenkins-test"],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}
