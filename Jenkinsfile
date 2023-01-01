pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Manual Approval') {
            steps {
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
            }
        }   
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deploy.sh'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
