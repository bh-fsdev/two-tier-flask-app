@Library('my-shared-lib') _
pipeline{
    agent { label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
                script{
                    git_clone.clone("https://github.com/bh-fsdev/two-tier-flask-app.git","master")
                }
              //git url: "https://github.com/bh-fsdev/two-tier-flask-app.git" , branch: "master"
            }
        }
        stage("Build"){
            steps{
                script{
                    dockerBuild.build("two-tier-flask-app")
                }
                //sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test"){
            steps{
                sh "echo 'Developer Testing this'"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                script{
                    docker_push.push("two-tier-flask-app","latest","dockerHubCred")
                }
            //   withCredentials([usernamePassword(
            //       credentialsId: "dockerHubCreds",
            //       passwordVariable: "dockerHubPass",
            //       usernameVariable: "dockerHubUser")]) {
            //       sh '''
            //       echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
            //       docker image tag two-tier-flask-app "$dockerHubUser/two-tier-flask-app:latest"
            //       docker push "$dockerHubUser/two-tier-flask-app:latest"
            //       '''
            //      }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
