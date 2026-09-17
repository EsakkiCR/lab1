pipeline{
    agent any
    stages{
        stage('Git Checkout'){
            steps{
                git branch:'master', url:'/opt/devops-project'
            }
        }
        
        stage('Docker Build'){
            steps{
                echo "Building docker Image"
                sh 'docker build -t devops_image:1.0 .'
            }
        }
        
        stage('Listing Images'){
            steps{
                echo "Listinig docker Image"
                sh 'docker images'
            }
        }
        
        stage('Remove Prerequsites'){
            steps{
                echo "Removing old containers"
                sh 'docker stop devops_container || exit 0'
                sh 'docker rm devops_container || exit 0'
            }
        }
        
        stage('Deploying'){
            steps{
                echo "Deploying Application"
                sh 'docker run -d --name devops_container -p 82:80 devops_image:1.0'
            }
        }
        
        stage('Verify Container'){
            steps{
                echo "Verifying Deployed containers status"
                sh 'docker ps -a'
            }
        }
    }
}
