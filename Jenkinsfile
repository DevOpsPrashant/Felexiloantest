pipeline {
    agent any
    
     environment {
        dockerImage = ''
        registry = "pras1804/flexiloan"
        registryCredential = 'dockerhub'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/DevOpsPrashant/Felexiloantest']]])
            }
        }
        stage('Build Docker image') {
            steps {
                script {
                dockerImage = docker.build registry
            }
        }
     }
        stage ('Uploading image to docker hub') {
            steps {
                script {
                      docker.withRegistry( '', registryCredential ) {
                      dockerImage.push()      
                      }
                }
            }
        }
        
        stage ('EKS deployment') {
            steps {
                kubernetesDeploy (
                    configs: 'Felexiloantest/flexiloank8sfile.yaml',
                    kubeconfigId: 'K8S',
                    enableConfigSubstitution: true
                    )
            }
        }
    }
}

