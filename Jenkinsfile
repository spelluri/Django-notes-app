pipeline {
    agent any 
    stages{
        stage("code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/spelluri/Django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps {
                echo "Building the code"
                sh "docker build -t my-note-app ."
            }
            
        }
        stage("Dockerhub Login") {
            steps {
                echo "Logging in to Dockerhub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                 echo "Print dockerhub username ${env.dockerhubUser}"
                  sh "docker tag my-note-app ${env.dockerhubUser}/my-note-app:$BUILD_NUMBER"
                  sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                  sh "docker push ${env.dockerhubUser}/my-note-app:$BUILD_NUMBER"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
