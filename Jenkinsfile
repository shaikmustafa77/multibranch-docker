pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t image3 .'
            }
        }

        stage('Tag') {
            steps {
                // Use the branch name as the Docker tag
                sh "docker tag image3 msvbhargav99/paytm:${env.BRANCH_NAME}"
            }
        }

        stage('Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub') {
                        sh "docker push msvbhargav99/paytm:${env.BRANCH_NAME}"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                // 1) Remove any existing container named 'movie-app'
                // 2) Start a fresh one, using the branch name as image tag
                sh '''
                    if docker ps -a --format '{{.Names}}' | grep -q '^movie-app$'; then
                      docker rm -f movie-app
                    fi
                '''
                sh "docker run -d --name movie-app -p 3333:80 msvbhargav99/paytm:${env.BRANCH_NAME}"
            }
        }
    }
}
