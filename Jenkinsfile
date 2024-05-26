pipeline {
    agent any
    stages {
        stage('auth interface') {
            steps {
                build job: 'auth-interface', wait: true
            }
        }
        stage('account interface') {
             steps {
                build job: 'account-interface', wait: true
            }
        }
        stage('build  auth') { 
            steps {
                sh 'mvn clean package'
            }
        }   
         
        stage('build image auth') {
            steps {
                script {
                    account = docker.build("esdrasgc/auth:${env.BUILD_ID}", "-f Dockerfile .")
                }
            }
        }
        stage('push image auth') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credential') {
                        account.push("latest")
                        account.push("${env.BUILD_ID}")
                       
                    }
                }
            }
        }

        stage('Deploy on k8s') {
            steps {
                sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT} --insecure-skip-tls-verify=true apply -f ./k8s/deployment.yaml --validate=false"
                sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT} --insecure-skip-tls-verify=true apply -f ./k8s/service.yaml --validate=false"
                
            }
        }

        
    }
}
