pipeline {
    agent any
    stages{
        stage('Git Checkout'){
            steps{
                git 'https://github.com/thejab811/Demo-counter-app.git'
            }
        }
        stage('Unit Testing'){
            steps{
                sh 'mvn test'
            }
        }

        stage('Integrating Testing'){
            steps{
                sh 'mvn verify -DskipUnittests'
            }
        }
        stage('maven build'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Static code analysis'){
            steps{
                script{
                withSonarQubeEnv(credentialsId: 'sonar-api') {
                    sh 'mvn clean package sonar:sonar'
                    }
 }


            }
            
        }
        stage('Quality gate status'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                }
            }
        }
        stage('upload jar file to nexus'){
            steps{
                script{
                    def readPomVersion = readMavenPom file: 'pom.xml'
                    def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "demoapp-snapshot" : "demoapp-release"                    
                    nexusArtifactUploader artifacts:
                    [
                        [
                        artifactId: 'springboot', 
                        classifier: '', file: 'target/Uber.jar', 
                        type: 'jar'
                        ]
                    ], 
                    credentialsId: 'nexus-auth', 
                    groupId: 'com.example', 
                    nexusUrl: '3.236.146.69:8081/', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: nexusRepo, 
                    version: "${readPomVersion.version}"
                }
            }
        }
        stage('Docker Image Build'){
            
            steps {
            
            script {
                sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                sh 'docker image tag $JOB_NAME:v1.$BUILD_ID thejaboddu/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker image tag $JOB_NAME:v1.$BUILD_ID thejaboddu/$JOB_NAME:latest'
                }
            }
        }
    }
}
