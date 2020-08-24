pipeline {
    agent any
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
                echo 'Testing....'
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
              sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/gentle-hollows-39973.git master:refs/remotes/heroku/master'
            }
          }
        }
    }
}