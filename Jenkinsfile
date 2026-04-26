pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build JAR') {
            steps {
                dir('complete') {
                    sh 'mvn clean package'
                }
            }
        }
    }
}
