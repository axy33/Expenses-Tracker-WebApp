@Library ("shared") _
pipeline{
    agent {label "agent-01"}
    stages{
        stage("hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        stage("code"){
            steps{
                script{
                clone("https://github.com/axy33/Expenses-Tracker-WebApp.git","main")
                }
            }
        }
        stage("build"){
            steps{
                echo "this is building stage"
                sh "docker compose up -d"
            }
        }
        stage("pushToDocker"){
            steps{
                echo "this is pusshing an image to docker"
                withCredentials([usernamePassword(
                'credentialsId':'dockerCred',
                passwordVariable: 'dockerhubpass',
                usernameVariable: 'dockerhubuser')]){
        sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
        sh "docker image tag expense-tracker-app:latest ${env.dockerhubuser}/expense-tracker-app:latest"
        sh "docker push ${env.dockerhubuser}/expense-tracker-app:latest"
                }
            }
        }
        stage("deploy"){
            steps{
                echo "this is deploy stage"
                sh "docker run -d -p 8080:8080 expense-tracker-app:latest"
            }
        }
    }
}
