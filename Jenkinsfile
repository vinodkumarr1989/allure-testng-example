pipeline {
    agent { label 'uitests' }
    stages {
        stage('Build') {
            steps {
                sh './mvnw -Dmaven.test.failure.ignore=true clean verify site'
                sh 'cp -r target/site/allure-maven-plugin /home/ubuntu/report/output/output-comdev'
            }
        }
         stage('Report') {
                    steps {
                        allure([includeProperties: false, jdk: '', properties: [], reportBuildPolicy: 'ALWAYS', results: [[path: '/home/ubuntu/report/output/output-comdev']] ])
                    }
                }
    }
    post {
        always {
            deleteDir()
        }
        failure {
            slackSend message: "${env.JOB_NAME} - #${env.BUILD_NUMBER} failed (<${env.BUILD_URL}|Open>)",
                    color: 'danger', teamDomain: 'qameta', channel: 'allure', tokenCredentialId: 'allure-channel'
        }
    }
}
