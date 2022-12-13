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
    }
}