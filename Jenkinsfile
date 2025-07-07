pipeline {
    agent any

    tools {
        maven "Maven-3.9.8"   // ✅ must match name configured in Jenkins
        jdk 'Java-17'      // ✅ optional, based on your project
    }

    environment {
        SONAR_TOKEN = credentials('SONARCLOUD_TOKEN')
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
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
