pipeline{
    agent any;
    
    stages{
        stage("Code clone"){
            steps{
                git url: "https://github.com/priyanshu6m/two-tier-flask-app", branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t my-app ."
            }
        }
        stage("test"){
            steps{
                echo "step 3"
            }
        }
        stage("push to docker hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "dockerhubcred",
                    passwordVariable: "dockerhubpass",
                    usernameVariable: "dockerhubUser")]){
                    sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubpass}"
                    sh "docker image tag my-app ${env.dockerhubUser}/my-app"
                    sh "docker push ${env.dockerhubUser}/my-app:latest"
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
