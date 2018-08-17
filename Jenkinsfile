pipeline {
    agent{ label 'master'}
    stages {
        stage ('Checkout') {
          steps {
            git 'https://github.com/effectivejenkins/spring-petclinic.git'
          }
        }
        stage('Build') {
            agent { docker 'maven:3.5-alpine' }
            steps {
                sh 'mvn clean package'
                junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Deploy') {
          steps {
            input 'Do you approve the deployment?'
            sh 'scp target/*.jar gfigueroa@193.95.165.194:/opt/pet/'
            sh "ssh gfigueroa@193.95.165.194 'nohup java -jar /opt/pet/spring-petclinic-1.5.1.jar &'"
          }
        }
    }
}
