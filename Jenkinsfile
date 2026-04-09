pipeline
{
  agent any

  environment
  {
    DOCKER_IMAGE="jordan0638/webapp"
  }


  stages
  {
    stage("Clone")
    {
      steps
      {
        git branch: "main",
        url: "https://github.com/Jordan0638/Midterm.git"
      }
    }

    stage('Build Docker Image') 
    {
      steps 
      {
        bat "docker build -t %DOCKER_IMAGE% ."
      }
    }

    stage("Docker Run")
    {
        steps
        {
            bat "docker run -d -p 8082:8080 %DOCKER_IMAGE%"
        }
    }

    stage('Push to DockerHub') 
    {
      steps 
      {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',usernameVariable: 'USER', passwordVariable: 'PASS')]) 
        {
            bat 'docker login -u %USER% -p %PASS%'
            bat "docker push %DOCKER_IMAGE%"
        }
      }
    }

    stage('Deploy to Kubernetes') 
    {
      steps 
      {
        bat 'kubectl apply -f deployment.yaml --validate=false'        
        }
    }
}
}