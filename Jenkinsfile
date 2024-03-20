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
        git branch: 'main', credentialsId: 'github', url: 'https://github.com/YourUsername/YourLiveAppRepo'
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
                stage("Git-repo deployment update - Trigger ArgoCD SYNC") {
      steps {
        script {
            withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
            sh """    
                    git config --global user.name "YourGithubUsername" 
                    git config --global user.email "YourGithubEmail" 
                    git add deploy.yaml 
                    git commit -m "Tags updated on deployment file"
                    git push https://github.com/YourUsername/YourLiveAppRepo main
               """
             }
      }
        }           
         
         } // stages ending
} // pipe line ending
}
