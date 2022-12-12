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
        
    }
}