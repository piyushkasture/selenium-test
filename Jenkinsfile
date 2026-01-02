pipeline {
    agent none   // pipeline itself not tied to one node

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
                    // add credentialsId if repo is private
                    // credentialsId: 'your-jenkins-credentials-id'
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
            agent any   // give post block a workspace
            steps {
                echo 'Archiving test reports'
                junit '**/target/surefire-reports/*.xml'
            }
        }
    }
}
