pipeline {
    agent {label 'agent'}
    environment{
      PROJECT_NAME = 'Spring-Petclinic'
      //UBUNTU_SSH_CRED = credentials('UBUNTU-SSH')
    }
    options { 
        timeout(time: 1, unit: 'HOURS')
        retry(2) 
        disableConcurrentBuilds()
    }
    tools {
      maven 'mvn-3.6'
    }
    triggers {
        cron('*/5 * * * *')
    }
    parameters {
        text(name: 'COMMENT', defaultValue: '', description: 'Write The Comment About The Job Why Are You Running....!!!')
        booleanParam(name: 'FORCE_DEPLOYMENT', defaultValue: true, description: 'Check This For Force Deployment From Scrach')
        choice(name: 'ENV', choices: ['main', 'gh-pages', 'springboot3','wavefront'], description: 'Pick The Needed Environment')
        
  }
  stages {
    stage('Check The Env') {
      when {
        branch 'springboot3'
      }
      input{
        message "Should we continue?"
        ok " yes we should....!"
        submitter "Alice,bob"
      }
      steps {
                 echo "Biography: ${params.COMMENT}"
                 echo "Toggle: ${params.FORCE_DEPLOYMENT}"
                 echo "${params.ENV} Present environment!"      
            }
        }
    stage('Approvel') {
      input{
        message "Should we continue?"
        ok " yes we should....!"
        submitter "Alice,bob"
        parameters {
           string(name: 'Software', defaultValue: '', description: 'What the component....?')
        }
      } 
      steps{
        echo "Hello ${params.Software}"
      }
      } 
    stage('branch') {
      when {
        beforeAgent true
        branch 'springboot3'
      }
    } 
    //stage('parallel steps') {
     //parallel
    //}   
    stage('Knowing About Project Name') {
      environment {
        PROJECT_NAME = "java"
      }
      steps {
        sh "echo ${PROJECT_NAME}"
        sh "env"
        sh "mvn compile && mvn package "
      }
    }      

      stage('Env Deploy') {
            when {
                branch 'wavefront'
                environment name: 'DEPLOY_TO', value: 'wavefront'
            }
            steps {
                echo 'Deploying'
            }
        }    
    stage('Message') {
      steps {
    
        sh("echo ${env.BUILD_ID}")
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
        // post {
        //   always{
        //     echo  "Hello"
        //   }
        // }
    
  }
}

