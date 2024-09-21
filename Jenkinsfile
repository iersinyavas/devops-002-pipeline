pipeline {
    agent any

    tools{
        jdk 'JDK17'
        maven 'Maven3'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mimaraslan/devops-002-pipeline']])
                bat 'mvn clean install'
                //sh 'mvn clean install'  macOS, Linux için
            }
        }

        stage('Unit Test') {
            steps {
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
                  bat 'echo Docker Image to Clean'
             }
        }
    }
}
