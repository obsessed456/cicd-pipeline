pipeline {
    agent any
    tools { nodejs 'node' }
     
     environment {
        IMAGE = "${env.BRANCH_NAME == 'main' ? 'nodemain' : 'nodedev'}"
        PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
    }

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
         }
         stage('Test') {
             steps { sh 'npm test'
             }
         }
         stage('Docker Build') {
             steps {
                 sh 'docker build -t $IMAGE:v1.0 .'
             }
         }
         stage('Deploy') {
             steps { 
                 sh 'docker rm -f $IMAGE || true'
                 sh 'docker run -d --expose $PORT -p $PORT:3000 $IMAGE:v1.0 .'
             }
         } 
    }
}  
