pipeline {
    agent any

    stages {
        stage('Build Artifact') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Unit Tests - JUnit and Jacoco') {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                    jacoco(execPattern: 'target/jacoco.exec')
                }
            }
        }

        stage('Print Environment Variables') {
            steps {
                sh 'printenv'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t yasiru1997/numeric-app:${GIT_COMMIT} ."
            }
        }

        stage('Docker Push') {
            steps {
                sh "docker push yasiru1997/numeric-app:${GIT_COMMIT}"
            }
        }
    }
}
