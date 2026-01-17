pipeline {
    agent any

    tools {
        maven 'maven-3.9.12'
    }

    stages {

        stage('Source Code Management') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Ravirudrayya/spring-petclinic.git'
            }
        }

        stage('Build the Code') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Archive the Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Publish Test Results') {
            steps {
                junit '**/target/surefire-reports/*.xml'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    sonar-scanner \
                      -Dsonar.projectKey=spring-petclinic \
                      -Dsonar.projectName=spring-petclinic \
                      -Dsonar.sources=src \
                      -Dsonar.java.binaries=target
                    '''
                }
            }
        }
    }
}

