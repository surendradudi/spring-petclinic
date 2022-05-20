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
        stage('checking_build_id & jenkins_url') {
            steps {
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            }
        }
       
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
        
    }
    
}
