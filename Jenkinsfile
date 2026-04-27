pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    environment {
        GROUP_ID = 'com.example'
        ARTIFACT_ID = 'springboot-app'
        VERSION = '1.0.0'
        REPOSITORY = 'maven-releases'
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

        stage('Upload to Nexus') {
            steps {
                script {
                    def jarFile = sh(
                        script: "ls complete/target/*.jar",
                        returnStdout: true
                    ).trim()

                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: 'host.docker.internal:8081',
                        groupId: env.GROUP_ID,
                        version: env.VERSION,
                        repository: env.REPOSITORY,
                        credentialsId: 'nexus-creds',
                        artifacts: [
                            [
                                artifactId: env.ARTIFACT_ID,
                                classifier: '',
                                file: jarFile,
                                type: 'jar'
                            ]
                        ]
                    )
                }
            }
        }
    }
}
