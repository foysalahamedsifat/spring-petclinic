pipeline {
    agent any

    triggers {
        cron('H/3 * * * 4') // Runs every 3 minutes on Thursdays
    }

    tools {
        maven 'Maven_3.8.5'  // Ensure you have a configured Maven tool in Jenkins
        jdk 'JDK_17'         // Ensure JDK 17 is installed and configured in Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/YOUR_GITHUB_USERNAME/spring-petclinic.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Run Tests with JaCoCo') {
            steps {
                sh 'mvn test'
                sh 'mvn jacoco:report'
            }
        }

        stage('Generate Artifact') {
            steps {
                sh 'mvn package'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Publish JaCoCo Report') {
            steps {
                jacoco execPattern: '**/target/jacoco.exec', 
                       classPattern: '**/target/classes',
                       sourcePattern: '**/src/main/java',
                       exclusionPattern: '**/test/**'
            }
        }
    }

    post {
        success {
            echo 'Build and Test Successful!'
        }
        failure {
            echo 'Build Failed!'
        }
    }
}
