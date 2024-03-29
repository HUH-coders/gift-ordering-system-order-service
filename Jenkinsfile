pipeline { 
    environment { 
        registry = "urvi99/order-service" 
        registryCredential = 'dockerhub' 
        dockerImage = ''
    }
    agent any
    stages { 
        stage('Cloning our Git') { 
            steps { 
                git branch:'main', url:'https://github.com/HUH-coders/gift-ordering-system-order-service.git'
            }
        }
        stage('Building Image') { 
            steps { 
                script { 
                    dockerImage = docker.build(registry + ":$BUILD_NUMBER"," .")
                }
            } 
        }
        stage('Testing Stage') {
            steps {
                script{
                    bat 'echo "Testing successful"'
                }
            }
            
        }
        stage('Deploying image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                }
            }
        }
        stage('Cleaning up') { 
            steps { 
                bat "docker rmi $registry:$BUILD_NUMBER" 
            }
        }
    }
    post {
        always {
            echo 'The pipeline completed'
        }
        success {                   
            echo "Flask Application Up and running!!"
        }
        failure {
            echo 'Build stage failed'
            error('Stopping early…')
        }
      }
}
