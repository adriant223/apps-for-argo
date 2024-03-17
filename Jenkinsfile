pipeline {
  agent {
    label "working-bee"
  }
  environment{
    APP_NAME = "my-demo-app"
  }
  stages {
    stage("del WS") {
      steps {
        cleanWs()  // Clean workspace before checkout
      }
    }

    stage("Git checkout") {
      steps {
        git branch: 'main', credentialsId: 'github', url: 'https://github.com/adriant223/jenk-full-pipeline'
      }
    }
        stage("Fetching new tags..") {
      steps {
        script {
          sh """    
                    cat deploy.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deploy.yaml
             """
        }
      }
        }
                stage("Update deploymet in Git repo") {
      steps {
        script {
          sh """    
                    git config --global user.name "atimis223"
                    git config --global user.email "atimis223@gmail.com
                    git add deploy.yaml
                    git commit -m "Tags updated on deployment file"
             """
             withCredentials([gitUsernamePassword(credentialsId: 'github-key', gitToolName: 'Default')])}
      }
        }           
         
         } // stages ending
} // pipe line ending
