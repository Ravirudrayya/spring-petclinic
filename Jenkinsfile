pipeline{
    agent any 
     tools {
        maven 'maven-3.9.12'
    }
    stages{
        stage('scource code management'){
            steps{
               git branch: 'main', url: 'https://github.com/Ravirudrayya/spring-petclinic.git'
            }
        }
        stage('Build the code'){
            steps
            {
                 sh 'mvn clean package'
            }
        }
        stage('Artifcate the code'){
            steps
            {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
       stage('Publish Test Result '){
            steps
            {
                 junit '**/target/surefire-reports/*.xml'
            }
        }
     stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    mvn sonar:sonar \
                    -Dsonar.projectKey=spring-petclinic \
                    -Dsonar.projectName=spring-petclinic \
                    -Dsonar.java.binaries=target
                    '''
                }
            }
        }
    
    }
}
