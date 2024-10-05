pipeline {
    agent any

    tools{
        jdk 'JDK17'
        maven 'Maven3'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/iersinyavas/devops-002-pipeline']])
                bat 'mvn clean install'
                //sh 'mvn clean install'  macOS, Linux için
            }
        }

        stage('Unit Test') {
            steps {
                //sh 'mvn test'
                bat 'mvn test'
                //sh 'echo docker Unit Test'
                bat 'echo docker Unit Test'
            }
        }

        stage('Docker Image') {
            steps {

                bat 'docker build -t iersinyavas/my-application:latest .'
                //sh 'docker build -t iersinyavas/my-application .'   macOS, Linux için
            }
        }

        stage('Docker Image to DockerHub') {
            steps {
                script{
                    withCredentials([string(credentialsId: 'dockerHub', variable: 'dockerHub')]) {
                        //bat 'docker login -u iersinyavas -p DOCKER_HUB'
                        bat 'echo docker login -u iersinyavas -p ${dockerHub}'
                        bat 'docker push iersinyavas/my-application:latest'
                    }
                }
            }
        }

        stage('Deploy Kubernetes') {
            steps {
              script{
                    kubernetesDeploy (configs: 'deployment-service.yml', kubeconfigId: 'kubernetes')
              }
            }
        }

        stage('Docker Image to Clean') {
             steps {
                 //sh 'docker rmi iersinyavas/my-application:latest'
                 //bat 'docker rmi iersinyavas/my-application:latest'
                 // 'docker image prune -f'
                 bat 'docker image prune -f'
             }
        }
    }
}
