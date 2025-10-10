pipeline {
    agent any

    stages {
        stage('Checkout code') {
            steps {
                echo 'Lấy code từ Git...'
                git branch: 'main', url: 'https://github.com/dung2609-lang/khoa-luan-next-jest-cicd.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Cài đặt gói Node...'
                bat 'npm install'
            }
        }

        // Stage SAST đã bị bỏ vì máy Windows chưa cài Sonar Scanner
        // stage('SAST - SonarQube') {
        //     steps {
        //         echo 'Chạy phân tích SAST với SonarQube...'
        //         withSonarQubeEnv('MySonarQube') {
        //             bat 'sonar-scanner.bat -Dsonar.projectKey=demo-web -Dsonar.sources=.'
        //         }
        //     }
        // }

        stage('SCA - Dependency Check') {
            steps {
                echo 'Quét thư viện bằng OWASP Dependency-Check...'
                bat 'dependency-check.bat --project demo --scan . --format HTML --out reports\\'
            }
        }

        stage('DAST - OWASP ZAP') {
            steps {
                echo 'Quét ứng dụng web bằng OWASP ZAP...'
                bat 'zap-cli.bat quick-scan --self-contained --start-options "-config api.disablekey=true" http://localhost:3000'
            }
        }

        stage('Deploy (Giả lập)') {
            steps {
                echo 'Triển khai thử nghiệm ứng dụng...'
            }
        }
    }

    post {
        always {
            echo 'Lưu báo cáo quét lại để xem sau...'
            archiveArtifacts artifacts: 'reports/**', fingerprint: true
        }
    }
}
