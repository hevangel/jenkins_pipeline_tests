pipeline {
    agent any 
    stages {
        stage('build') {
            steps {
                sh 'touch a.txt'
                sh 'python3 --version'
                sh 'date >> a.txt'
            }
        }
        stage('deploy') {
            sh 'git commit -am 'daily check in'
            sh 'git push origin master'
        }
     }
 }
