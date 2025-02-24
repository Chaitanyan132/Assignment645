pipeline{
  agent any
  environment {
    DOCKERHUB_CREDENTIALS=credentials('dockerhub')
  }
  stages{
    stage('Build'){
      steps{
        sh 'rm -rf *.var'
        sh 'jar -cvf StudentSurvey.war -C "StudentSurvey/src/main/webapp" .'
        sh 'docker build -t chaitanya203030/studentsurveyform:latest .'
      }
    }
    stage('Login'){
      steps{
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW |docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage("Push image to docker hub"){
      steps{
        sh 'docker push chaitanya203030/studentsurveyform:latest'
      }
    }
    stage("deploying on k8")
    {
      steps{
        sh 'kubectl set image deployment/deploymentagain container-0=chaitanya203030/studentsurveyform:latest -n default'
        sh 'kubectl rollout restart deploy deploymentagain -n default'
      }
    }
  }
  post{
    always{
      sh 'docker logout'
    }
  }
}
