pipeline {
  agent any
  environment {
    DOCKER_ID = "mathieu101"
    DOCKER_IMAGE = "datascientestapi"
    DOCKER_TAG = "v.${BUILD_ID}.0"
  }
  stages{
    stage('Building'){
      steps{
        sh 'pip install -r requirements.txt'
      }
    }
      stage('Testing'){
        step{
          sh 'python -m unittest'
        }
      }
      stage('Deploying'){
        step{
          script {
            sh'''
            docker rm -f jenkins
            docker build -t $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG .
            docker run -d -p 8000:8000 --name jenkins $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG
            '''
          }
        }
      }
      stage('confirm'){
        step{
          input{
            message "Voulez vous déployer le code sur la branche main"
          }
        }
      }
      stage('pushing and merging'){
        parallel{
          stage('pushing'){
            environment{
              DOCKERHUB_CREDENTIALS = credentials('docker_jenkins')
            }
            steps{
              sh 'echo $DOCKERHUB_CREDENTIALS_PSW |docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
              sh 'docker push $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG'
            }
          }
          stage('merging'){
            step{
              echo 'merging done'
            }
          }
        }
      }
  }
  post{
    always{
      sh 'docker logout'
    }
  }
}
