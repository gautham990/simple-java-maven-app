pipeline {
    agent node1 
    tools {
        maven "maven363"    //Verison of Maven defined in global config
    } 
    options {
        timeout(10)     //Restrict the job to 10mins
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '3', numToKeepStr: '3') //Discard old builds
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
            }
        }  
    }     
    post {
        always {
            deleteDir() //Clean the workspace post completion
            }
        }
}
