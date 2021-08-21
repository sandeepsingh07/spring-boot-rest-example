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
                    jacoco()
                }
            }
        }
        
        stage('SonarQube Anaysis') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.host.url="http://192.168.0.203:9000" -Dsonar.login=$sonar_auth -DdeployAtEnd=false'
               
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
