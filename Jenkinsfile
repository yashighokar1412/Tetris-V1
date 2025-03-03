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

        stage("Docker Build") {
            steps {
                script {
                    sh "docker build -t tetrisv1 ."
                }
            }
        }

        stage("Docker Tag") {
            steps {
                script {
                    sh "docker tag tetrisv1 yashthedocker/tetrisv1:latest"
                }
            }
        }

        stage("Docker Push") {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', url: 'https://index.docker.io/v1/') {
                        sh "docker push yashthedocker/tetrisv1:latest"
                    }
                }
            }
        }

        stage("Docker Images") {
            steps {
                script {
                    sh "docker images"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withAWS(credentials: 'aws', region: 'ap-south-1') {
                        withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', serverUrl: '']]) {
                            sh "kubectl apply -f deployment.yml"
                        }
                    }
                }
            }
        }
    }
}
