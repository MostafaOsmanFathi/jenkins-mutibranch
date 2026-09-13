pipeline {
    agent any
 
    environment {
        APP_NAME = 'nodejs-app' 
        REPO_URL = "https://github.com/MohamedMagdy840/jenkins-repo.git"
    }

    stages{

        stage('Getting Repo files') {
            steps {
                git branch: "${GIT_BRANCH}", credentialsId: 'jenkins', url: "${REPO_URL}"
            }
        }
         stage('Build Docker Image') {
            steps {
                script {
                    sh """
                        docker build -t mostafaosmanfathi/${APP_NAME}:${BUILD_NUMBER} .
                    """
                }
            }
        }

        stage('login'){
            steps{
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) 

                {
                     sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                                            -u "$DOCKER_USERNAME" \
                                            --password-stdin                    
                                            
                        '''
                }

               
            }
        }

         stage('Push Docker Image') {
            steps {
                script {
                    sh """
                        docker push mostafaosmanfathi/${APP_NAME}:${BUILD_NUMBER}
                    """
                }
            }
        }
        

    }

}
