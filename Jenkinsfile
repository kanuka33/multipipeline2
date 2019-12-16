pipeline {
environment {
    registry = "ravidocker438/pipelinetestprod"
    registryCredential = 'dockerhub'
	}
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf dockertest2'
            sh 'git clone https://github.com/ravimedi/dockertest2.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /var/lib/jenkins/workspace/pipeline2/dockertest2'
            sh 'cp  /var/lib/jenkins/workspace/pipeline2/dockertest2/* /var/lib/jenkins/workspace/pipeline2'
            sh 'docker build -t ravidocker438/pipelinetestprod:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push ravidocker438/pipelinetestprod:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://192.168.10.63:2375 stop prodwebapp1 || true'
            sh    'docker -H tcp://192.168.10.63:2375 run --rm -dit --name prodwebapp1 --hostname prodwebapp1 -p 8000:80 ravidocker438/pipelinetestprod:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://192.168.10.63:8000'
          }
        }

    }
}
