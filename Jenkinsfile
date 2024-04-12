pipeline {
    agent any
    stages {
        stage('Build Auth') {
            steps {
                build job: 'auth', wait: true
            }
        }
        stage('Build account') {
             steps {
                build job: 'account', wait: true
            }
        }
        stage('Build') { 
            steps {
                sh 'mvn clean package'
            }
        }   
         
        stage('Build Image') {
            steps {
                script {
                    account = docker.build("fernandowi55/auth:${env.BUILD_ID}", "-f Dockerfile .")
                }
            }
        }
        stage('Push Image') {
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