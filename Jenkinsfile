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
                sh "Developer Testing this"
            }
        }
        stage("Push to Docker Hub"){
            steps{
              withCredentials([usernamePassword(
                credentialsId: "dockerHubCreds",
                passwordVar: "dockerHubPass",
                usernameVar: "dockerHubUser")]){
                  sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass} "
                  sh "docker image tag two-tier-flask-app${env.dockerHubUser}/two-tier-flask-app"
                  sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
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
