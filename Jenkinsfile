pipeline {
    agent any

    environment {
        dockerhub=credentials('dockerhub')
    }

    stages {
        stage('Init') {
            steps {
                echo 'Initializing..'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                sh 'echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin'
            }
        }
        stage('Stage 1') {
            steps {
                echo 'Building..'
                sh 'docker build -t $dockerhub_USR/static_page .'
            }
        }
        stage('Stage 2') {
            steps {
                echo 'Publishing..'
                echo 'Publishing image to DockerHub..'
                sh 'docker push $dockerhub_USR/static_page:latest'
            }
        }
        stage('Cleanup') {
            steps {
                echo 'Cleaning..'
                sh 'docker images --filter "dangling=true" -q --no-trunc | docker rmi'
                sh 'docker images -q | xargs --no-run-if-empty docker rmi'
            }
        }
    }
}
