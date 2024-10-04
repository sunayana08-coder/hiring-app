pipeline {
    agent any

        environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        
       
     stage('Docker Build') {
            steps {
                sh "docker build . -t sunayana09/hiring-app:$BUILD_NUMBER"
            }
        }
        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'Hub_Psswd', usernameVariable: 'Docker_ID')]) {
                    sh "docker login -u sunayana09 -p ${Hub_Psswd}"
                    sh "docker push sunayana09/hiring-app:$BUILD_NUMBER"
                }
            }
        }
        stage('Checkout K8S manifest SCM'){
            steps {
              git branch: 'main', url: 'https://github.com/sunayana08-coder/Hiring-app-argocd.git'
            }
        } 
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                   withCredentials([usernamePassword(credentialsId: 'Github', passwordVariable: 'Git_Passwd', usernameVariable: 'Git_User')]) { 
                        sh '''
                        cat /var/lib/jenkins/workspace/$JOB_NAME/dev/deployment.yaml
                       echo "Before sed command:"
                    sh 'cat /var/lib/jenkins/workspace/docker-push/dev/deployment.yaml'
                    sh 'sed -i s/5/4/g /var/lib/jenkins/workspace/docker-push/dev/deployment.yaml'
                    echo "After sed command:"
                    sh 'cat /var/lib/jenkins/workspace/docker-push/dev/deployment.yaml'

                        git add .
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://$Git_User:$Git_Passwd@github.com/sunayana08-coder/Hiring-app-argocd.git main
                        '''                        
                      }
                  }
            }   
        }
            }
} 
