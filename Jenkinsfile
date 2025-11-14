pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                git branch: 'main', url: 'https://github.com/Brindha-V-C/Todo-app-CI-CD'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t brindhavc559/my-node-app:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                    docker push brindhavc559/my-node-app:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Update Deployment File') {

            environment {
                GIT_REPO_NAME = "springboot-app-cicd"
                GIT_USER_NAME = "Brindha-V-C"
            }

            steps {
                withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                        git config user.email "brindhavc15@gmail.com"
                        git config user.name "Brindha V C"
                        BUILD_NUMBER=${BUILD_NUMBER}
                        sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" deploy/deploy.yaml
                        git add deploy/deploy.yaml
                        git commit -m "Update deployment image to version ${BUILD_NUMBER}"
                        git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
                    '''
                }
            }
    }
}
