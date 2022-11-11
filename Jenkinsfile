pipeline {
    agent {
        kubernetes {
            yamlFile 'agentpod.yaml'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Clone repository') { 
            steps { 
                script {
                    checkout scm
                }
            }
        }
        stage('Build') { 
            steps { 
                container('docker') {
                    script {
                        app = docker.build("eks-example", "--network host -f build/Dockerfile .")
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                container('docker') {
                    script{
                        docker.withRegistry('https://102076985746.dkr.ecr.ap-northeast-2.amazonaws.com', 'ecr:ap-northeast-2:aws-credentials') {
                            app.push("latest")
                            app.push("${env.BUILD_NUMBER}")
                        }
                    }
                }
            }
        }
    }
}
