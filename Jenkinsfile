pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONARCLOUD_TOKEN')     // Jenkins credential ID for SonarCloud token
        MAVEN_HOST = 'jenkins@172.31.86.33'              // Remote Maven host
    }

    stages {
        stage('Build') {
            steps {
                sshagent(['c55758a5-d427-462f-ae84-7786d7442c0c']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $MAVEN_HOST '
                        cd /home/maven/app &&
                        mvn clean install'
                    """
                }
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                sshagent(['c55758a5-d427-462f-ae84-7786d7442c0c']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $MAVEN_HOST '
                        cd /home/maven/app &&
                        mvn sonar:sonar \\
                          -Dsonar.login=$SONAR_TOKEN \\
                          -Dsonar.host.url=https://sonarcloud.io \\
                          -Dsonar.organization=jb-org \\
                          -Dsonar.projectKey=maven-project'
                    """
                }
            }
        }

    }
}
