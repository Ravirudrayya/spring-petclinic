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
      stage('Upload to Nexus') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '192.168.40.135:8081',
                    repository: 'maven-snapshots',
                    credentialsId: 'nexus',
                    groupId: 'org.springframework.samples',
                    version: '4.0.0-SNAPSHOT',
                    artifacts: [
                        [
                            artifactId: 'spring-petclinic',
                            classifier: '',
                            file: 'target/spring-petclinic-4.0.0-SNAPSHOT.jar',
                            type: 'jar'
                        ]
                    ]
                )
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
                    script {
                        def scannerHome = tool 'SonarQube-Scanner'
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                          -Dsonar.projectKey=spring-petclinic \
                          -Dsonar.projectName=spring-petclinic \
                          -Dsonar.sources=src \
                          -Dsonar.java.binaries=target
                        """
                    }
                }
            }
        }
    }
}

