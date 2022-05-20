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
         string(name: 'PERSON', defaultValue: 'Mr/Mrs Java', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: 'In This Project I installed Some Required Pacakages', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'ENV', choices: ['main', 'gh-pages', 'springboot3','wavefront'], description: 'Pick The Needed Branch')

        
  }
  stages {
    stage('Check The Env') {
      steps {
               
                 echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                 echo "${"params.ENV"} Present environment!"

                
            }
        }  

      stage('Env Deploy') {
            when {
                branch 'wavefront'
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
    
  }
}

