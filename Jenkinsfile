pipeline {
    agent any
    triggers { pollSCM('* * * * *') }
    stages {
        stage('SourceCode') {
            steps {
                git branch: 'mybranch', url: 'https://github.com/surendradudi/spring-petclinic.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Archive and Test Results') {
            steps {
               junit '**/surefire-reports/*.xml'
               archiveArtifacts artifacts: '**/*.war', followSymlinks: false
            }
        }
    }
}