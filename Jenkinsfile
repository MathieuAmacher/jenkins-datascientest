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
  }
}
