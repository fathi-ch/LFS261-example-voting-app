pipeline {
    agent any 

    stages {
        stage("one") {
            steps {
                echo 'step 1'
                sleep 3
            }
        }
        stage("two") {
            steps {
                echo 'step 2'
                sleep 9
            }
        }
        stage("three") {
            steps {
                echo 'step 3'
                sleep 5
            }
        }
    } 

    post {
        always {
            echo 'This pipeline is completed.'
            slackSend (
                channel: '#your-channel',
                color: 'good',
                message: "Build ${env.JOB_NAME} #${env.BUILD_NUMBER} completed with status ${currentBuild.currentResult}",
                tokenCredentialId: 'slack-token'
            )
        }
    }
}
