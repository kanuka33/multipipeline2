pipeline {
environment {
    registry = "ravidocker438/fetureapp2"
    registryCredential = 'dockerhub'
	}
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf multipipeline2'
            sh 'git clone https://github.com/ravimedi/multipipeline2.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            
            sh 'docker build -t ravidocker438/fetureapp2:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push ravidocker438/fetureapp2:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://192.168.10.197:2375 stop fetureapp2 || true'
            sh    'docker -H tcp://192.168.10.197:2375 run --rm -dit --name fetureapp2 --hostname fetureapp2 -p 8000:80 ravidocker438/fetureapp2:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://192.168.10.197:8000'
          }
        }

    }
}
