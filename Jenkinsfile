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
                trivy_fs()
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
                docker_push("dockerHubCreds","two-tier-flask-app")
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
            email_notification('Successful', 'Good News: Your Build was successful!')
        }
        failure {
            email_notification('Failed', 'Bad News: Your Build has failed!')
        }
}
}
