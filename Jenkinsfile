pipeline{
	agent any
	stages{
		stage('Config Repo'){
			steps{
				git branch:'main', url:'/opt/devops-project'
			}
		}
		stage('Docker Build'){
			steps{
				echo "Building Docker Image"
				sh 'docker build -t testimage:1.0 .'
			}
		}
		stage('Pre Check Deployment'){
			steps{
				echo "Stope and Remove existing containters.."
				sh 'docker stop testcontainer || true'
				sh 'docker rm testcontainer || true'
			}
		}
		stage('Docker Deploymet'){
			steps{
				echo "Deploying application"
				sh 'docker run -d --name testcontainer -p 82:80 testimage:1.0'
			}
		}
		stage('Verify Container'){
			steps{
				echo "Verfying docker depoyment.."
				sh 'docker ps -a'
			}
		}
	}
}
