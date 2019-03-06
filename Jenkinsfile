// Template Jenkinsfile for Python CI/CD
// url: https://github.com/INWT/inwt-templates/blob/master/jenkins/python_ci.Jenkinsfile
// Author: Matthäus Deutsch
pipeline {
    agent {label 'vm3'}
    options { disableConcurrentBuilds() }
    stages {
        stage('Preparation of Virtualenv') {
            steps {
            // copy credentials to the right folder
            // install package to virtualenv
                sh '''
                pipenv install --dev
                '''
            }
        }
        stage('Tests') {
            // pytest tests within the virtualenv
            steps {
                sh '''
                pipenv run pytest --verbose
                '''
            }
        }
    }
}
