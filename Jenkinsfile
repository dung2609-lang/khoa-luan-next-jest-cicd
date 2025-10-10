pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/<your-username>/demo-web.git'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('SAST - SonarQube') {
            steps {
                withSonarQubeEnv('MySonarQube') {
                    sh 'sonar-scanner -Dsonar.projectKey=demo-web -Dsonar.sources=.'
                }
            }
        }

        stage('SCA - Dependency Check') {
            steps {
                sh 'dependency-check.sh --project demo --scan . --format HTML --out reports/'
            }
        }

        stage('DAST - OWASP ZAP') {
            steps {
                sh 'zap-cli quick-scan --self-contained --start-options "-config api.disablekey=true" http://localhost:3000'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'reports/**', fingerprint: true
        }
    }
}
