pipeline {
    agent any 
    tools {
        maven "maven363"
    } 
    stages {
        stage('Build') {
            steps {
                sh "mvn -B clean package -DskipTests"
            }    
        } 
        stage('Test') {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit "target/surefire-reports/*.xml"
                }
        }   }
    } 
}
