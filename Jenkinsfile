pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        stage('Build Maven'){
            steps{
                // checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/iChancetek/devops-automation']]])
                   checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'Github', url: 'https://github.com/iChancetek/devops-automation']])
                sh 'mvn clean install'
            }  
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t ichancetek/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                  // withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) 
                 withDockerRegistry(credentialsId: 'dockerhub', url: 'https://registry.hub.docker.com/')  {
                     sh 'docker login -u "ichancetek"  -p "Ch@ncetek869219" docker.io'
                  }

                   sh 'docker push ichancetek/devops-integration:0.0.1'
                }
            }
        }
        // stage('Deploy to k8s'){
        //     steps{
        //         script{
        //             kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
        //         }
        //     }
        }
    }
