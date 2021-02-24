node
{
    stage("checkout code")
    {
        git 'https://github.com/hareesh-bot/nodejs-app-mss.git'
    }
    stage("Build")
    {
        sh "npm install"
    }
    
    stage("Execute SonarQube Report")
    {
        sh "npm run sonar"
    }
   
    stage("Build Docker Image")
    {
        sh "docker build -t hareeshdock/node-web-app ."
    }
    stage("push docker image into docker Hub")
    {
        withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Pwd')])
        {
            sh "docker login -u hareeshdock -p ${Docker_Hub_Pwd}"
            sh "docker push hareeshdock/node-web-app "
        }
    }
    stage("deploy docker image into deployment server")
    {
        def dockerRun = "docker run -d -p 8080:8080 --name containername  hareeshdock/node-web-app "
        sshagent(['nodejs']) 
        {
            sh "ssh -o StrictHostKeyChecking=no ubuntu@3.139.106.222"
           
            sh "ssh ubuntu@3.139.106.222 ${dockerRun}"
        }
    }
    
}
