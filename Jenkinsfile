pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONARCLOUD_TOKEN')  // Add this in Jenkins credentials
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'  // or your build command
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}


