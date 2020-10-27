node{
    stage('Pull Source Code From GITHUB'){
      git 'https://github.com/sanjaydahiya332/cloudknowledge.git'
    }
    
    stage('Build Docker File'){
        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID sd171991/$JOB_NAME:v1.$BUILD_ID'
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID sd171991/$JOB_NAME:latest'
    }
    
    stage('Push Image To Docker HUB'){
        withCredentials([string(credentialsId: 'dockerhubpassword', variable: 'dockerhubpassword')]) {
    // some block
    sh 'docker login -u sd171991 -p ${dockerhubpassword}'
    sh 'docker image push sd171991/$JOB_NAME:v1.$BUILD_ID'
    sh 'docker image push sd171991/$JOB_NAME:latest'
    sh 'docker image rmi $JOB_NAME:v1.$BUILD_ID sd171991/$JOB_NAME:v1.$BUILD_ID sd171991/$JOB_NAME:latest'
    
}
       
    }
    
    stage('Deployment of Docker container'){
        def dockerRun = 'docker run -p 8000:80 -d --name cloudknowledges  sd171991/scripted-pipeline-demo:latest'
        def dockerrm = 'docker container rm -f cloudknowledges'
        def dockerimagerm = 'docker image rmi  sd171991/scripted-pipeline-demo'
        
        sshagent(['hostpassword']) {
    // some block
    sh "ssh -o StrictHostKeyChecking=no  ec2-user@172.31.40.135 ${dockerrm}"
    sh "ssh -o StrictHostKeyChecking=no  ec2-user@172.31.40.135 ${dockerimagerm}"
    sh "ssh -o StrictHostKeyChecking=no  ec2-user@172.31.40.135 ${dockerRun}"
    
}
    }
    
    
}
