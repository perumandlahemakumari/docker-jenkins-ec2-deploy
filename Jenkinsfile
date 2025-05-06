pipeline {
    agent any
    stages{
        stage ('code') {
            steps {
                git 'https://github.com/suneelprojects/docker-site.git'
            }
        }
        stage ('Build') {
            steps {
                sh 'docker build -t Hema:v1 html'
            }
        }
        stage ('Deploy') {
            steps {
                sh 'docker run -itd --name cycle -p 8089:80 Hema:v1'
            }
        }
    }
}
