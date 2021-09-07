pipeline {
    agent any
     tools{
        jdk 'java'
        maven 'maven'
    }
   // agent { label 'master' }
    stages {
        stage ('Checkout') {
          steps {
            git 'https://github.com/mhali922/spring-petclinic.git'
          }
        }
        stage('Build') {
           // agent { docker 'maven:3.5-alpine' }
            steps {
                sh 'mvn clean package'
                junit '**/target/surefire-reports/TEST-*.xml'
               // archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('publish to Nexus') {
                steps{
                     nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'spring-petclinic', 
                            classifier: '', 
                            file: 'target/petclinic-1.5.1.jar', 
                            type: 'jar'
                        ]
                    ], 
                    credentialsId: 'nexus3', 
                    groupId: 'org.springframework.samples', 
                    nexusUrl: '172.31.23.232:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'spring-petclinic', 
                    version: '1.5.1'
                    }
            }
        
        /**
        stage('Deploy') {
          steps {
            input 'Do you approve the deployment?'
            sh 'scp target/*.jar jenkins@192.168.50.10:/opt/pet/'
            sh "ssh jenkins@192.168.50.10 'nohup java -jar /opt/pet/spring-petclinic-1.5.1.jar &'"
          }
        }
        **/
    }
}
