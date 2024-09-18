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

            // sh 'mvn clean install'
             bat 'mvn clean install'
            }
        }


        stage('Docker Image') {
            steps {
            // sh 'docker build  -t iersinyavas/my-application  .'
            // bat 'docker build  -t iersinyavas/my-application  .'
               bat 'docker build  -t iersinyavas/my-application:latest  .'
            }
        }


        stage('Docker Image to DockerHub') {
            steps {
              script{
                    withCredentials([string(credentialsId: 'dockerHub', variable: 'dockerHub')]) {

                        // bat 'docker login -u iersinyavas -p DOCKERHUB_TOKEN'

                         // sh 'echo docker login -u iersinyavas -p ${dockerhub}'
                          bat 'echo docker login -u iersinyavas -p ${dockerhub}'

                        // sh 'docker image push  iersinyavas/my-application:latest'
                          bat 'docker image push  iersinyavas/my-application:latest'

                    }
                }
            }
        }


       stage('Deploy Kubernetes') {
            steps {
              script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml', kubeconfigId: 'kubernetes')
              }
            }
        }


    }
}
