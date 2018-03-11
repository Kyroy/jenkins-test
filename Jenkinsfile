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
               setBuildStatus(context, "In progress...", "PENDING");
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
           setBuildStatus(context, "Success", "SUCCESS");
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

// Updated to account for context
void setBuildStatus(String context, String message, String state) {
  context = context ?: "ci/jenkins/build-status"
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/my-org/my-repo"],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: context],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}
