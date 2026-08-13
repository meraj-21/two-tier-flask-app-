pipeline {
    agent {label 'dev'}
    
    stages {
        stage("Code clone") {
            steps {
                git url: "https://github.com/meraj-21/two-tier-flask-app-.git", branch: "master"
            }
        } 
        
        stage("Build") {
            steps {
                sh 'sudo chown -R $USER:$USER /home/ubuntu/jenkins/workspace/demo-cicd/mysql-data'
                sh "docker build -t two-tier-flask-app ."
            }
        }
        
        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }
        
        stage("Test") {
            steps {
                echo "Developer / tester tests likh ke dega ....."
            }
        }
        
        stage("Deploy") {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
