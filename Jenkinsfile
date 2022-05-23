pipeline {
    agent  {
        label 'agent'
        }
    environment{
      PROJECT_NAME = 'Spring-Petclinic'
  
    }
    options { 
        timeout(time: 1, unit: 'HOURS')
        retry(2) 
        disableConcurrentBuilds()
    }
    tools {
      maven 'mvn-3.6'
    }
    //triggers {
      //  cron('*/5 * * * *')
    //}
    parameters {
        text(name: 'COMMENT', defaultValue: '', description: 'Write The Comment About The Job Why Are You Running....!!!')
        booleanParam(name: 'FORCE_DEPLOYMENT', defaultValue: true, description: 'Check This For Force Deployment From Scrach')
        choice(name: 'ENV', choices: ['main', 'gh-pages', 'springboot3','wavefront'], description: 'Pick The Needed Environment')
        
  }
  stages {
    stage('Check The Env & Approval') {
      input{
        message "Should we continue?"
        ok " yes we should....!"
        submitter "Alice,bob"
        parameters {
           string(name: 'Project', defaultValue: ' ', description: 'What the project....?')
        }
      }
      steps {
                 echo "COMMENT: ${params.COMMENT}"
                 echo "FORCE_DEPLOYMENT: ${params.FORCE_DEPLOYMENT}"
                 echo "${params.ENV} Present environment!" 
                 echo "${params.Project}"
   
            }
        }
    stage('Test') {
            steps {
                sh 'java --version'
                sh 'docker info'
                sh 'docker images'
                sh 'docker-compose up
            }
        }
    stage('Knowing About Project Name') {
      steps {
        sh "echo ${PROJECT_NAME}"
        sh "env"
      }
    }       
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
                 archiveArtifacts artifacts: '**/target/*.jar', followSymlinks: false
               
            }
        }
    stage('reporting') {
      steps {
                junit testResults: 'target/surefire-reports/*.xml'
              
            }

        }
  }
    post {
        always {
            emailext body: 'A Test EMail', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
        }
    }
}

