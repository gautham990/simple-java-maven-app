pipeline {
    agent {
        label "node1"
    }
    tools {
        maven "maven363"    //Verison of Maven defined in global config
    } 
    options {
        timeout(10)     //Restrict the job to 10mins
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '3', numToKeepStr: '3') //Discard old builds
    }
    stages {
        stage("Build") {
            steps {
                sh "mvn -B clean package -DskipTests"
            }    
        } 
        stage("Test") {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit "target/surefire-reports/*.xml" //Publish test reports on Jenkins dashboard
                }
            }
        }  
        stage("SonarQube analysis") {
            steps {
                withSonarQubeEnv("sonarqube") {
                    sh "mvn sonar:sonar"  //Sonarqube analysis
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: "HOURS") {   // Wait for Quality gate 
                    waitForQualityGate abortPipeline: true
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
