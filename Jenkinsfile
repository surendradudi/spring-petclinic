pipeline {
    agent {label 'agent'}
    options { 
        timeout(time: 1, unit: 'HOURS')
        retry(2) 
    }
    triggers {
        cron('0 * * * *')
    }
    stages {
        stage('Source Code') {
            steps {
                git url: 'https://github.com/surendradudi/spring-petclinic.git', 
                branch: 'main'
            }

        }
        stage('Build the Code') {
            steps {
                sh script: 'mvn clean package'
               
            }
        }
        stage('reporting') {
            steps {
                junit testResults: 'target/surefire-reports/*.xml'
            }

        }
        stage('Test') {
            steps {
                sh 'make check || true' 
                junit 'target/*.xml' 
            }
        }
    }
    
}
