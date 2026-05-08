pipeline {
    agent any
    
    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/TuyenKimHuynh/8.2CDevSecOps.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
        }
        
        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }
        
        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }
        
        stage('SonarCloud Analysis') {
            steps {
                script {
                    def sonarScanner = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv(installationName: 'SonarCloud', credentialsId: 'SONAR_TOKEN') {
                        
                        bat """
                            cd nodejs-goof
                            ${sonarScanner}/bin/sonar-scanner.bat ^
                                -Dsonar.projectKey=TuyenKimHuynh_8.2CDevSecOps ^
                                -Dsonar.organization=tuyenkimhuynh ^
                                -Dsonar.host.url=https://sonarcloud.io ^
                                -Dsonar.login=%SONAR_TOKEN% ^
                                -Dsonar.sources=. ^
                                -Dsonar.exclusions=node_modules/**,test/** ^
                                -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info
                        """
                    }
                }
            }
        }
    }
}
