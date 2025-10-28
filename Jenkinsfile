pipeline{
    
    agent { label "dev"};
    
    stages{
        stage("code"){
            steps{
                git url:"https://github.com/anshdeploys/two-tier-flask-app.git", branch:"master"
            }
        }
        stage("build"){
            steps{
                sh "docker build -t two-tier-flask-app:latest ."
            }
        }
        stage("test"){
            steps{
                echo "testing ...."
            }
        }
        stage("dockerhub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerhubCreds", 
                    usernameVariable:"dockerhubUser", 
                    passwordVariable:"dockerhubPass"
                )]){
                sh "docker login -u ${dockerhubUser} -p ${dockerhubPass}"
                sh "docker image tag two-tier-flask-app ${dockerhubUser}/two-tier-flask-app:latest"
                sh "docker push ${dockerhubUser}/two-tier-flask-app:latest"
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker-compose up -d --build two-tier-flask-app:latest"
            }
        }
    }
}
