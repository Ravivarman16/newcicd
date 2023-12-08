pipeline {
    agent any

    stages {

        stage('Login into DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${docker}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin docker.io"
                }
                
                stage ('changing the file permission') {
                    steps {
                        sh 'chmod +x build.sh'
                        sh 'chomod +x deploy.sh'
                    }
                }
                
                stage ('executing the file') {
                    steps {
                        sh './build.sh'
                        sh './deploy.sh'
                        }
                    }
                }
            }
        }
    }
