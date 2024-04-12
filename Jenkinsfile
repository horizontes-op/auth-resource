pipeline {
    agent any
    stages {
        stage('Build Account') {
            steps {
                script {
                    // Executa o job "account" e armazena o status de retorno
                    def accountStatus = build job: 'account', propagate: false
                    if (accountStatus == 'SUCCESS') {
                        echo 'Job "account" executado com sucesso.'
                    } else {
                        error 'Falha ao executar o job "account".'
                    }
                }
            }
        }
        stage('Build Auth-Resource') {
            steps {
                build job: 'auth', wait: true
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