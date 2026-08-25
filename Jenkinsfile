@Library("Shared") _
pipeline {
    agent {label 'dev'}
    
    stages {
        stage("Code clone") {
            steps {
                clone("https://github.com/meraj-21/two-tier-flask-app-.git","master")
            }
        } 
        stage("Trivy File System Scan"){
            steps{
                sh "trivy fs . -o results.json"
            }
        }
        stage("Build") {
            steps {
                //sh 'sudo chown -R $USER:$USER /home/ubuntu/jenkins/workspace/demo-cicd/mysql-data'
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
post {
    success {
        emailext (
            to: 'meraj11ahmed@gmail.com',
            subject: 'Build Successful',
            body: 'Good News: Your Build was successful!' 
        )
    }
    failure {
        emailext (
            to: 'meraj11ahmed@gmail.com',
            subject: 'Build Failed',
            body: 'Bad News: Your Build has failed!'
        )
    }
}
}
