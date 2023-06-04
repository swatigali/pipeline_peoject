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
        
        stage("SonarQube_check_quality"){
            
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonartoken') {
                      sh 'mvn sonar:sonar'
                      
                   }
                    def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    sh "mvn clean install"  
                }
               }
             }  
        
        stage('Docker Build'){
            steps{
                sh 'docker build -t swatigali/main_docker_n_image .'
            }
        }
        
        stage('Docker Push'){
            steps{
                withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerpush')]) {
                sh 'docker login -u swatigali -p ${dockerpush}'
                }
                 sh 'docker push swatigali/main_docker_n_image'
            }
           
        }
    }
}
