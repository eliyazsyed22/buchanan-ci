
pipeline{
    
    agent any 

    environment{
        APP_NAME = "public.ecr.aws/p5u5p5h0/buchananecr"
        IMAGE_TAG = "${BUILD_ID}"
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
                   sh 'docker image build -t ${APP_NAME}:latest .'
                    //sh 'docker build -t public.ecr.aws/p5u5p5h0/buchananecr:${env.BUILD_NUMBER} .'
                   //sh 'docker tag buchananecr:buchananlatest public.ecr.aws/p5u5p5h0/buchananecr:buchananlatest'
                }
            }
        }
        stage('Docker Image Push to ECR'){

            steps{
                script{
                   sh 'docker push ${APP_NAME}:latest'
                }
            }
        }
        stage('Trigger Update Manifest') {
            steps{
                    echo "triggering Update manifest Job"
                    build job: 'argocd-update-manifest', parameters: [string(name: 'DOCKERTAG', value: '${IMAGE_TAG}')]
            }
        }
        /*stage('Update k8 deployment file'){
            steps{
                script{
                    sh 'sed -i 's|${APP_NAME}.*|${APP_NAME}:${IMAGE_TAG}|g' test.yaml'
                }
            }
        }*/
    }       
}
