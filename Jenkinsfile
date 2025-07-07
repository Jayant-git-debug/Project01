pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONARCLOUD_TOKEN')
        MAVEN_HOST = 'jenkins@172.31.86.33'
    }

    stages {
        stage('Build') {
            steps {
                sshagent(['c55758a5-d427-462f-ae84-7786d7442c0c']) {
                    sh "ssh -o StrictHostKeyChecking=no $MAVEN_HOST 'cd /path/to/your/project && mvn clean install'"
                }
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                sshagent(['c55758a5-d427-462f-ae84-7786d7442c0c']) {
                    sh "ssh -o StrictHostKeyChecking=no $MAVEN_HOST 'cd /path/to/your/project && mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN}'"
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
