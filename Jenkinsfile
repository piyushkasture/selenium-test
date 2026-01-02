pipeline {
    agent none   // pipeline ko ek hi node pe lock mat karo

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
            agent any
            steps {
                git branch: 'main',
                    url: 'https://github.com/mobilitytrackinginfra/selenium-test.git'
            }
        }

        stage('Build & Test in Parallel') {
            parallel {
                stage('Node1 Tests') {
                    agent { label 'node1' }
                    steps {
                        bat 'mvn clean test'
                    }
                }
                stage('Node2 Tests') {
                    agent { label 'node2' }
                    steps {
                        bat 'mvn clean test'
                    }
                }
                stage('Node3 Tests') {
                    agent { label 'node3' }
                    steps {
                        bat 'mvn clean test'
                    }
                }
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
