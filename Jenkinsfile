pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('mytoken') // Replace 'sonar-token-id' with your actual SonarQube token ID in Jenkins credentials
    }

    stages {
        stage('Clone React App') {
            steps {
                git 'https://github.com/saba-2002/react-a-saba.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Scan') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') { // Replace 'SonarQube' with the name of your SonarQube installation in Jenkins
                        sh '''
                        sonar-scanner \
                            -Dsonar.projectKey=react-project \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=sqa_4dbf447f5e04be71a21fc21c7088f2d37b3ebc0e
                        '''
                    }
                }
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy React App') {
            steps {
                sh 'rsync -avz -e "ssh -i /var/lib/jenkins/.ssh/id_rsa" /var/lib/jenkins/workspace/mysonargithub/build/ ubuntu@3.7.66.118:/var/www/html '
            }
        }
    }

    post {
        always {
            cleanWs() // Clean up the workspace after the build
        }
    }
}
