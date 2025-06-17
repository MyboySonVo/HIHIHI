pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17.0.12'  // đường dẫn thật trên máy Jenkins
        PATH = "${env.JAVA_HOME}\\bin;${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'gradlew.bat build'
            }
        }

        stage('Test') {
            steps {
                bat 'gradlew.bat test'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/build/libs/*.jar', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def jarFiles = findFiles(glob: 'build/libs/*.jar').findAll { !it.name.contains('-plain') }

                    if (jarFiles.isEmpty()) {
                        error "❌ No valid JAR file found to deploy"
                    }

                    def jarPath = jarFiles[0].path
                    echo "🚀 Deploying JAR: ${jarPath}"
                    bat "java -jar \"${jarPath}\""
                }
            }
        }
    }

    post {
        success {
            echo '✅ Jenkins pipeline completed successfully!'
        }
        failure {
            echo '❌ Jenkins pipeline failed.'
        }
    }
}
