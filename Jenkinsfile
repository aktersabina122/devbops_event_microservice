node{
    
    stage('GitHub Checkout'){
        git branch: 'master', credentialsId: 'git-creds', url: 'https://github.com/anishmoktan/devbops_blog_microservice'
    }
    
    stage('DevBops Blog Test'){
        sh 'python3 test.py'
    }
    
    stage('Docker Image Build'){
        sh 'docker build -t anishmoktan/devbops_blog .'
    
    }

    stage('Docker Image Push'){

        withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]) {
            sh "docker login -u anishmoktan -p ${dockerHubPwd}"
    
            }
        
        sh 'docker push anishmoktan/devbops_blog'
    
    }

    stage('Run Docker Container on Private EC2'){
        def dockerRm = 'docker rm -f app_blog'
        def dockerRmI = 'docker rmi anishmoktan/devbops_blog'
        def dockerRun = 'docker run -p 8091:80 -d --name app_blog anishmoktan/devbops_blog'
        sshagent(['docker-server']) {
            sh "ssh -o StrictHostKeyChecking=no ec2-user@172.25.11.85 ${dockerRm}"
            sh "ssh -o StrictHostKeyChecking=no ec2-user@172.25.11.85 ${dockerRmI}"
            sh "ssh -o StrictHostKeyChecking=no ec2-user@172.25.11.85 ${dockerRun}"
           
        }
    
    }   
}
