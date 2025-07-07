pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONARCLOUD_TOKEN')
        MAVEN_HOST = 'jenkins@172.31.86.33'
        APP_DIR = '/home/maven/app'
        GIT_REPO = 'https://github.com/Jayant-git-debug/Project01.git'
    }

    stages {
        stage('Prepare') {
            steps {
                sshagent(['c55758a5-d427-462f-ae84-7786d7442c0c']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no \$MAVEN_HOST '
                        rm -rf \$APP_DIR &&
                        git clone \$GIT_REPO \$APP_DIR'
                    """
                }
            }
        }

        stage('Build') {
            steps {
                sshagent(['c55758a5-d427-462f-ae84-7786d7442c0c']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no \$MAVEN_HOST '
                        cd \$APP_DIR &&
                        mvn clean install'
                    """
                }
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                sshagent(['c55758a5-d427-462f-ae84-7786d7442c0c']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no \$MAVEN_HOST '
                        cd \$APP_DIR &&
                        mvn sonar:sonar -Dsonar.login=\$SONAR_TOKEN'
                    """
                }
            }
        }
    }
}
