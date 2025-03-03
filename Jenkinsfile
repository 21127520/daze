pipeline {
    agent any
    
    stages {

        stage('Prepare Environment') {
            steps {
                echo "Ensuring Gradle wrapper is executable..."
                sh 'chmod +x gradlew'
            }
        }

        stage('Build APK') {
            steps {
                echo "Building APK using Gradle..."
                sh './gradlew assembleDebug'
            }
        }

        stage('Check APK Path') {
            steps {
                script {
                    def apkPath = "${WORKSPACE}/app/build/outputs/apk/debug/app-debug.apk"
                    if (fileExists(apkPath)) {
                        echo "✅ APK file found: ${apkPath}"
                        sh "ls -la ${apkPath}"  // In ra thông tin file APK
                    } else {
                        error "❌ APK file not found: ${apkPath}"
                    }
                }
            }
        }

        stage('Q-Mast Scan') {
            steps {
                script {
                    def apkPath = "${WORKSPACE}/app/build/outputs/apk/debug/app-debug.apk"
                    echo "Scanning APK file: ${apkPath}"
                    kwSubmit filePath: apkPath, platform: 'android'
                }
            }
        }
    }
}