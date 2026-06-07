pipeline{
  agent any

    environment {
    KUBECONFIG = "/var/lib/jenkins/.kube/config"
  }
  stages{
    stage("github repo clone"){
      steps{
        git branch: "main",
          url: "https://github.com/Ayush-200/express-test-app.git"
        echo "cloning the github repo"
      }
      
    }

    stage("docker login"){
      steps{
        withCredentials([usernamePassword(credentialsId: "docker-cred", passwordVariable: "docker_pass", usernameVariable: "docker_username")]){
          sh 'echo $docker_pass | docker login -u $docker_username --password-stdin' 
          echo "docker login successfull"
        }
      }
    }

    stage("image build"){
      steps{
        sh 'docker build -t ayushbhatia123/express-app:latest .'
      }
    }

    stage("image push"){
      steps{
        sh 'docker push ayushbhatia123/express-app:latest'
      }
    }

    stage("docker image run"){
      steps{
        sh 'docker pull ayushbhatia123/express-app:latest'
        sh 'docker run -d -p 3000:3000 ayushbhatia123/express-app:latest'
      }
    }

    stage("kubernetes deployment"){
        steps{
            sh "kubectl apply -f deployment.yml"
            sh "kubectl get pods"
            sh "kubectl apply -f service.yml"
            sh "kubectl get svc"
        }
    }
  }
  
}