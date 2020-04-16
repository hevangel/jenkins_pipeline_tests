pipeline {
    agent any
    triggers {
        cron('H 0 * * 1-5')
    }
    parameters {
        booleanParam(name: 'PUSH', defaultValue: false, description: 'Push to github')
    }
    stages {
        stage('build') {
            steps {
                echo 'build phase'
                echo "workspace: ${env.WORKSPACE} on ${env.JENKINS_URL}"
                sh 'python3 -m venv --system-site-packages python_venv'
                withPythonEnv("${WORKSPACE}/python_venv/") {
                    sh 'pip3 install -r requirements.txt'
                    sh 'python3 test.py'
                }
                sh 'touch a.txt'
                sh 'date >> a.txt'
            }
        }
        stage('test') {
            steps {
                echo 'test phase'
            }
        }
        stage('archive') {
            steps {
                echo 'archive phase'
                junit allowEmptyResults: true, testResults: 'test_results.xml'
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
                archiveArtifacts 'a.txt'
            }
        }
        stage('deploy') {
            when {expression {return params.PUSH}}
            steps {
                echo 'deploy phase ha ha'
                sh 'git add -A'
                sh 'git commit -am "check in"'
                sshagent (['git-jenkins.hevangel.com']) {
                    sh 'git push origin HEAD:master'
                }
            }
        }
    }
    post {
        always {
            echo 'One way or another, I have finished'
            // deleteDir() /* clean up our workspace */
        }
        success {
            echo 'I succeeeded!'
            // emailext body: 'jenkins test', subject: 'jenkins test', to: 'hevangel@gmail.com'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'I failed :('
        }
        changed {
            echo 'Things were different before...'
        }
        aborted {
            echo 'I am aborted'
        }
        fixed {
            echo 'I am fixed'
        }
        regression {
            echo 'I am regression'
        }
        unsuccessful {
            echo 'I am unsuccessful'
        }
        cleanup {
            echo 'clean up at the end'
        }
    }
 }
