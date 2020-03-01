node
{
    stage('SCM Checkout'){
        git credentialsId: 'gitcreds', url: 'https://github.com/javahometech/my-app'
    }

    stage('Mvn Package'){
        def mvnHome = tool name: 'maven-3', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh label: '', script: "${mvnCMD} clean package"
    }

    stage('Build Docker Image'){
        sh 'docker build -t psurabh/my-app:2.0.0 .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'dockerhubpasswd', variable: 'Dockerhubpassword')]) {
        sh "docker login -u psurabh -p ${Dockerhubpassword}"         
        }
        sh 'docker push psurabh/my-app:2.0.0'
    }

    stage('Deploy Docker Image'){
        def dockerRun = 'docker run -d -p 8080:8080 --name myapp psurabh/my-app:2.0.0'
        sshagent(['SSHmaady']) {
        // some block      
           sh "ssh -o StrictHostKeyChecking=no ec2-user@13.239.26.236 ${dockerRun}"
        }
    }
}
