pipeline {
    agent any
    environment {

        EMAIL_BODY =
        """
            <p>EXECUTED: Job <b>\'${env.JOB_NAME}:${env.BUILD_NUMBER})\'</b></p>
            <p>
            View console output at
            "<a href="${env.BUILD_URL}">${env.JOB_NAME}:${env.BUILD_NUMBER}</a>"
            </p>
            <p><i>(Build log is attached.)</i></p>

        """

        EMAIL_SUBJECT_SUCCESS = "Status: 'SUCCESS' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'"
        EMAIL_SUBJECT_FAILURE = "Status: 'FAILURE' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'"
        EMAIL_RECEPIENT = 'mcherotichsitienei@gmail.com'
    }
    tools {
        nodejs "Node-Build"
        }
    stages {
        stage('build') {
            steps {
                sh 'npm install'
            }
        }
        stage('test') {
            steps {
                echo 'npm test'
            }
         }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
        stage('Deploy to Heroku') {
            steps {
                withCredentials([usernameColonPassword(credentialsId: 'heroku', variable: 'HEROKU_CREDENTIALS' )]){
                sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/limitless-meadow-81845.git HEAD:master'                
                }
            } 
        }
        stage('Slack Notification'){
            steps {
                slackSend color: "#337AFF", message: "Build Started :see_no_evil: ${env.JOB_NAME} ${env.BUILD_NUMBER} <${env.BUILD_URL}|-Open Jenkins>"
            }
        }
    }
        post {
        success {
            emailext attachLog: true,
                body: EMAIL_BODY,
                subject: EMAIL_SUBJECT_SUCCESS,
                to: EMAIL_RECEPIENT
            slackSend color: "#33FF3C", message: "Build Successful :sunglasses: ${env.JOB_NAME} ${env.BUILD_NUMBER} <https://limitless-meadow-81845.herokuapp.com/|-Open Heroku>"
        }

        failure {
            emailext attachLog: true,
                body: EMAIL_BODY,
                subject: EMAIL_SUBJECT_FAILURE,
                to: EMAIL_RECEPIENT
            slackSend color: "danger", message: "Build Failed :worried: ${env.JOB_NAME} ${env.BUILD_NUMBER} <${env.BUILD_URL}|-Open>"
             }
        }
}