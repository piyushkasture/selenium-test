pipeline {
    agent node1

    options {
        disableConcurrentBuilds()
    }

    tools {
        maven 'Maven'
        jdk 'Java'
    }

    environment {
        MAVEN_OPTS = '-Dmaven.test.failure.ignore=false'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mobilitytrackinginfra/selenium-test.git'
            }
        }

        stage('Build & Test') {
            steps {
                bat 'mvn clean test'
            }
        }
    }

    post {
        success {
            echo 'Selenium tests executed successfully'
        }

        unstable {
            echo 'Some Selenium tests failed'
        }

        failure {
            echo 'Build or test execution failed'
        }

        always {
            echo 'Archiving test reports'
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
