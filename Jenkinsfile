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
         stage('Push') {
             steps {
                 withCredentials([usernamePassword(credentialsId: 'dockerhub',
                     usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
                     sh 'echo $DH_PASS | docker login -u "$DH_USER" --password-stdin'
                     sh 'docker tag $IMAGE $DH_USER/$IMAGE:v1.0'
                     sh 'docker push $DH_USER/$IMAGE:v1.0'
                 }
             }
         }
         stage('Trigger Deploy') {
             steps {
                 script {
                     def job = env.BRANCH_NAME == 'main' ? 'Deploy_to_main' : 'Deploy_to_dev'
                     build job: job, wait: false
                 }
             }    
         }   
    }    
}    
