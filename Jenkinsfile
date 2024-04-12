pipeline {
    agent any
    stages {
        stage('build auth interface') {
            steps {
                build job: 'auth', wait: true
            }
        }
        stage('build account interface') {
             steps {
                build job: 'account push', wait: true
            }
        }
        stage('build aluno interface') { 
            steps {
                sh 'mvn clean package'
            }
        }   
         
        stage('build image aluno') {
            steps {
                script {
                    account = docker.build("fernandowi55/auth:${env.BUILD_ID}", "-f Dockerfile .")
                }
            }
        }
        stage('push image aluno') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credential') {
                        account.push("latest")
                        account.push("${env.BUILD_ID}")
                       
                    }
                }
            }
        }

        
    }
}