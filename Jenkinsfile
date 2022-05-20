pipeline {
    agent {label 'agent'}
    options { 
        timeout(time: 1, unit: 'HOURS')
        retry(2) 
    }
    triggers {
        cron('0 * * * *')
    }
    parameters {
        choice(name: 'ENV', choices: ['main', 'gh-pages', 'springboot3','wavefront'], description: 'Pick The Needed Branch')
  }
  stages {
    stage('Check The Env') {
      steps {
                echo "${params.ENV} Present environment!"
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
    
    stage('Test') {
      steps {
                sh 'make check'
            }
        }
    
    // post {
    //     always {
    //         junit '**/target/*.xml'
    //     }
    //     failure {
    //         mail to: surendradudi331@gmail.com, subject: 'The Pipeline failed :('
    //     }
        
    // }
    
  }
}

