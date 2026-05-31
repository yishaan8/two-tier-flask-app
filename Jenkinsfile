pipeline {
    agent {
        label "dev"
    }

    stages {

        stage('Code Clone') {
            steps {
                git url: 'https://github.com/yishaan8/two-tier-flask-app', branch: 'master'
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t two-tier-flask-app . "
            }
        }

        stage('Test') {
            steps {
                echo 'Testing the application'
            }
        }
        
        stage('push to docker hub'){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",
                passwordVariable:"dockerHubPass",
                usernameVariable:"dockerHubUser"
                )]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                sh "docker compose up -d --build"
            }
        }
    }
post {
        success {
            script {
                emailext from: 'yishaan8@gmail.com',
                subject: "Build success",
                body: 'Build success',
                to: 'yishaan8@gmail.com',
            }
        }
  failure {
            script {
                emailext from: 'yishaan8@gmail.com',
                subject: "Build unsuccessful",
                body: 'Build unsuccessful',
                to: 'yishaan8@gmail.com',
            }
        }
}
}
