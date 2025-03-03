pipeline {
    agent any
    environment {
        SONAR_HOME = tool "sonar"
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yashighokar1412/Tetris-V1.git'
            }
        }

        stage('Sonar Quality Check') {
            steps {
                withSonarQubeEnv(credentialsId: 'sonar', installationName: 'sonar') {
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=sonar -Dsonar.projectKey=sonar"
                }
            }
        }
    }
}
