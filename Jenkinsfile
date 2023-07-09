pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
               git branch: 'main', url: 'https://github.com/pawankahurke/jenkins/'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t hackerboypk/cicd-e2e:${BUILD_NUMBER} .
                    '''
                }
            }
        }
        

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    docker login -u="hackerboypk" -p="Pkhacker@1" docker.io
                    echo 'Push to Repo'
                    docker push hackerboypk/cicd-e2e:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
               git branch: 'main', url: 'https://github.com/pawankahurke/deploy.git'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    
                        sh '''
                        cat deploy.yaml
                        sed -i '' "s/16/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push git branch: 'main', url: 'https://github.com/pawankahurke/deploy.git' HEAD:main
                        '''                        
                    
                }
            }
        }
    }
}
