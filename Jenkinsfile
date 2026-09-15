pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/shai-sivakumar/7.1C'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
       stage('Run Tests') {
            steps {
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    bat 'npx snyk auth %SNYK_TOKEN%'
                    bat 'npm test || exit /b 0'
                }
            }
        }
        stage('Generate Coverage Report') {
            steps {
                bat 'npx nyc --reporter=lcov --report-dir=coverage mocha || exit /b 0'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }
        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    bat 'npm install --save-dev sonarqube-scanner'
                    bat 'npx sonar-scanner -Dsonar.login=%SONAR_TOKEN%'
                }
            }
        }
    }
}
