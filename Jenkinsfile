pipeline{
    
    agent { label "dev"};
    
    stages{
        stage("code"){
            steps{
                git url:"https://github.com/anshdeploys/two-tier-flask-app.git", branch:"master"
                
            }
        }
        stage("test"){
            steps{
                echo "under proccess"
                
            }
        }
        stage("build"){
            steps{
                sh "docker build -t two-tier-flask-app:latest ."
                
            }
        }
        stage("dockerhub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerhubCreds", 
                    usernameVariable: "dockerUser", 
                    passwordVariable: "dockerPass"
                )]){
                sh "docker login -u ${dockerUser} -p ${dockerPass}"
                sh "docker image tag two-tier-flask-app ${dockerUser}/two-tier-flask-app:latest"
                sh "docker push ${dockerUser}/two-tier-flask-app:latest"
                }
            }    
        }
        stage("deploy"){
            steps{
                sh "docker-compose up -d --build"
            }
        }
    }
}        
