
pipeline{
    
    agent any 

    environment{
        AWS_ACCOUNT_ID="p5u5p5h0"
        AWS_DEFAULT_REGION="us-east-1"
        IMAGE_REPO_NAME="buchananecr"
        REPOSITORY_URI="${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/eliyazsyed22/buchananCIandCD.git'
                }
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Maven Build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage('Elastic Container Registry Login'){
            
            steps{
                script{
                   sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/p5u5p5h0'
                }
            }
        }
        stage('Docker Image Build'){
            
            steps{
                script{
                   sh 'docker image build -t buchananecr:v1.$BUILD_ID .'
                    //sh 'docker build -t buchananecr:${env.BUILD_NUMBER} .'
                   //sh 'docker tag buchananecr:buchananlatest public.ecr.aws/p5u5p5h0/buchananecr:buchananlatest'
                }
            }
        }
        stage('Docker Image Push to ECR'){

            steps{
                script{
                   sh 'docker push public.ecr.aws/p5u5p5h0/buchananecr:v1.$BUILD_ID'
                }
            }
        }  
           
    }       
}
