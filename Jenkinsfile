pipeline{
    agent any
    
    tools {
        maven 'mymaven'
            }
    
    stages{
        
        stage('Git Clone'){
            
            steps{
                git branch: 'main', url: 'https://github.com/swatigali/pipeline_peoject.git'
            }
        }
        stage('Build'){
            steps{
                sh 'mvn install'
            }
        }
        stage('Docker Build'){
            steps{
                sh 'docker build -t swatigali/myjavaproject10may23 .'
            }
        }
         stage('Docker Push'){
            steps{
                withCredentials([string(credentialsId: 'docker_hub', variable: 'dockerpush')]) {
                sh 'docker login -u swatigali -p ${dockerpush}'
                }
                 sh 'docker push swatigali/myjavaproject10may23'
            }
    }
}    
}
