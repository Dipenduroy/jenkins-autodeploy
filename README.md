# Autodeploy Git Projects using jenkins

## Prerequisites
- Jenkins
- Git
- GitFTP

## Jenkins pipeline
```
pipeline {
    agent any
    stages {
        stage('Deploy') {
            steps {
                git 'https://github.com/Dipenduroy/jenkins-autodeploy.git'
                //Change your ftp credentials and deployment path
                //Need to run below command only once to push all files to the server. Refer : https://github.com/git-ftp/git-ftp 
                sh 'git ftp init --user your_ftp_username --passwd your_ftp_password ftp://panel.freehosting.com/public_html/jenkins/ftp'
                //After pushing all the files for the first time, Comment above git ftp init command
                
                //Below command moves only the necessary files after the second committ in the repository
                sh 'git ftp push --user your_ftp_username --passwd your_ftp_password ftp://panel.freehosting.com/public_html/jenkins/ftp'
                echo 'Auto deployment was successful.'
            }
        }
    }
    post {
        always {
            emailext body: 'Jenkins completed a build for the project : Jenkins Auto Deploy ', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Jenkins Auto Deploy Demo Project'
        }
    }
}
```
