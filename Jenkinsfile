pipeline {
    agent any 

    stages {
        stage("Sonarqube") {
            when {
                branch 'master'
            }
            environment {
                sonarpath = tool 'SonarScanner'
            }
            steps {
                echo 'Running Sonarqube Analysis..'
                withSonarQubeEnv('sonar-instavote') {
                    sh """
                        ${sonarpath}/bin/sonar-scanner \
                          -Dsonar.organization=fathi-ch \
                          -Dsonar.projectKey=fathi-ch_LFS261-example-voting-app \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=https://sonarcloud.io
                    """
                }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

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
        stage('voteintegration'){agentanywhen{changeset"**/vote/**"branch'master'}steps{echo'RunningIntegrationTestsonvoteapp'dir('vote'){sh'shintegration_test.sh'}}}
    } 

    post {
        always {
            echo 'This pipeline is completed.'
            slackSend (
                channel: '#all-ci-lab',
                color: 'good',
                message: "Build ${env.JOB_NAME} #${env.BUILD_NUMBER} completed with status ${currentBuild.currentResult}",
                tokenCredentialId: 'slack1'
            )
        }
    }
}
