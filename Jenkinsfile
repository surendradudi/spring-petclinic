pipeline {
    agent any //{
        //label 'agent'
        //label 'prod'
        //}
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
        text(name: 'COMMENT', defaultValue: 'This About Java Application ', description: 'Write The Comment About The Job Why Are You Running....!!!')
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
           string(name: 'Project', defaultValue: 'Spring-Petclinic', description: 'What the project....?')
        }
      }
      steps {
                 echo "COMMENT: ${params.COMMENT}"
                 echo "FORCE_DEPLOYMENT: ${params.FORCE_DEPLOYMENT}"
                 echo "${params.ENV} Present environment!" 
                 echo "${params.Project}"
   
            }
        }
    stage('Deploy') {
            steps {
                echo 'Deploying'
            }
        }  
    stage('Test') {
            steps { 
                sh 'sudo apt-get update'
                sh 'sudo apt-get install openjdk-11-jdk -y '
                sh 'sudo apt-get install maven -y '
                sh 'mvn --version'
                sh 'mvn clean package'
                sh 'sudo apt-get install docker.io -y '
                sh 'sudo docker info'
                sh 'sudo docker build -t spc .'
                sh 'sudo docker rmi d0e1132d5e44 69e1710ca296 ba7fdd536c6f bd54ea63328b   e7ab51511f55 f716c4e5ba92 75681d40c35d d8e3863bc758  72451e707d77' 
                sh 'sudo docker images'
                sh 'ls'
                sh 'sudo docker run -it -d -p 8085:8080 spc'
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
    
}

