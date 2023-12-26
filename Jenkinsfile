pipeline {
    agent any

    environment {
        DOCKER_BFLASK_IMAGE = 'your-docker-registry/your-flask-image'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Build the Docker image and tag it
                    sh 'docker build -t my-flask .'
                    sh "docker tag my-flask ${DOCKER_BFLASK_IMAGE}"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    // Run tests inside the Docker container
                    sh 'docker run my-flask python -m pytest app/tests/'
                }
            }
        }
        stage('Deploy') {
            stage('Login into DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin docker.io"
                }
            }
        }
    }
    }

    post {
    always {
       
        script {
            sh 'docker rm -f mypycont || true' // Remove the container, ignore failure if it doesn't exist
            sh 'docker run --name mypycont -d -p 3000:5000 my-flask'
        }

        
        emailext to: "jravi1605@gmail.com",
                 subject: "Notification mail from Jenkins",
                 body: "CI/CD pipeline completed successfully."
    }
}
}

