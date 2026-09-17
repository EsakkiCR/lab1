pipeline{
	agent any
	stages{
		stage('Docker Build'){
			steps{
				echo "Building Docker Image"
				sh 'docker build -t testimage:1.0 .'
			}
		}
		stage('Docker Deploymet'){
			steps{
				
				sh '''
				if [ -n "$(docker ps -a --filter "name = testcontainer" --format "{{.Names}}")"]; then
					echo "Container exist.. Stopping and Removing it."
					docker stop testcontainer
					docker rm testcontainer
				else
					echo "No Container exist, Nothing to remove"
				fi

				docker run -d --name testcontainer -p 82:80 testingimage:1.0
				'''
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
