pipeline{
    agent { label "dev2"};
    
    stages{
        stage("Code Clone"){
            steps{
              git url: "https://github.com/bh-fsdev/two-tier-flask-app.git" , branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test"){
            steps{
                sh "echo 'Developer Testing this'"
            }
        }
        stage("Push to Docker Hub"){
            steps{
              withCredentials([usernamePassword(
                  credentialsId: "dockerHubCreds",
                  passwordVariable: "dockerHubPass",
                  usernameVariable: "dockerHubUser")]) {
                  sh '''
                  echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
                  docker image tag two-tier-flask-app "$dockerHubUser/two-tier-flask-app:latest"
                  docker push "$dockerHubUser/two-tier-flask-app:latest"
                  '''
                 }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
