pipeline {     

    agent any

    stages {

        stage('Init') {

            steps {

                sh'''
                docker network create jenk-network || echo "Network already exists"
                docker stop flask-app || echo "flask-app not running"
                docker stop nginx || echo "nginx not running"
                docker rm flask-app || echo "flask-app not running"
                docker rm nginx || echo "nginx not running"
                '''

            }

        }
        
        stage('Build') {

            steps {

                sh'''
                docker build -t s90hef/flask-jenk:latest -t s90hef/flask-jenk:v${BUILD_NUMBER} .
                docker build -t s90hef/nginx-jenk:latest -t s90hef/nginx-jenk:v${BUILD_NUMBER} ./nginx
                '''

            }

        }

        stage('Push') {

            steps {

                sh'''
                docker push s90hef/flask-jenk:latest
                docker push s90hef/flask-jenk:v${BUILD_NUMBER}
                docker push s90hef/nginx-jenk:latest
                docker push s90hef/nginx-jenk:v${BUILD_NUMBER}
                '''

            }

        }

        stage('Deploy') {

            steps {

                sh'''
                docker run -d --name flask-app --network jenk-network s90hef/flask-jenk
                docker run -d -p 80:80 --name nginx --network jenk-network s90hef/nginx-jenk
                '''

            }

        }

        stage('Clean') {

            steps {

                sh'''
                docker system prune -f
                '''

            }

        }

    }

}