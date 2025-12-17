pipeline{
    agent { label 'dev-agent' };
    stages{
        stage("code"){
            steps{
                echo "code clone start "
                git url:"https://github.com/NiravKavar/two-tier-flask-app.git", branch:"dev"
                echo "code clone end ho gaya"
            }
        }
        
       
        
        stage("build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        
        
        stage("test"){
            steps{
                echo "Running basic static tests inside venv"
                sh '''
                    python3 tests/test_basic.py
                '''
            }
        }
        stage("push to dockerhub"){
            steps{
                withCredentials([usernamePassword(
                credentialsId:"dockerCred",
                usernameVariable:"dockerHubUser",
                passwordVariable:"dockerHubPass"
                )]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
