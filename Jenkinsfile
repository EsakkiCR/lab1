pipeline{
        agent any
        parameters{
                        booleanParam(name: 'ROLLBACK', defaultValue: false, description: 'Check to execute rollback'),
                        string(name: 'TARGET_VERSION', defaultValue: '', description: 'Version tag to rollback to')
        }

        stages{
                stage('Deploy or Rollback') {
                    steps{
                        script{
                            if (params.ROLLBACK) {
                                echo "Rolling back to version: ${params.TARGET_VERSION}"
                                sh '''
                                        if [ -n "$(docker ps -a --filter "name = testcontainer" --format "{{.Names}}")" ]; then
                                                echo "Container exist.. Stopping and Removing it."
                                                docker stop testcontainer
                                                docker rm testcontainer
                                        else
                                                echo "No Container exist, Nothing to remove"
                                        fi

                                        docker run -d --name testcontainer -p 82:80 testimage:${params.TARGET_VERSION}
                                    '''

                            } else {
                                echo "Proceeding with standard deployment..."
                                    echo "Building Docker Image"
                                    sh 'docker build -t testimage:$BUILD_NUMBER .'
                                    sh '''
                                        if [ -n "$(docker ps -a --filter "name = testcontainer" --format "{{.Names}}")" ]; then
                                                echo "Container exist.. Stopping and Removing it."
                                                docker stop testcontainer
                                                docker rm testcontainer
                                        else
                                                echo "No Container exist, Nothing to remove"
                                        fi

                                        docker run -d --name testcontainer -p 82:80 testimage:$BUILD_NUMBER
                                    '''

                            }
                        }
                    }
                }
        }
}
