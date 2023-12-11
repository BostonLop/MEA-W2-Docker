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
                docker build -t s90hef/flask-jenk .
                docker build -t s90hef/nginx-jenk ./nginx
                '''

            }

        }

        stage('Push') {

            steps {

                sh'''
                docker push s90hef/flask-jenk
                docker push s90hef/nginx-jenk
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