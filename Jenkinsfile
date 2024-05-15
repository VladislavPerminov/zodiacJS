pipeline {
    agent {
        label 'agent_node'
    }
    
    environment {
        PAT_DOCKERHUB = credentials('PAT_Dockerhub')
    }
 
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/VladislavPerminov/zodiacJS.git'
            }
        }
        stage('Build') {
            steps {
                sh '''
                    npm install
                    npm run build
                '''
            }
        }
        stage('Test') {
            steps {
                sh 'npm run test'
            }
        }
        stage('Delivery') {
            steps {
                sh 'docker login -u steppenwol -p ${PAT_DOCKERHUB}'
                sh 'docker build . -t steppenwol/zodiac:${BUILD_ID}'
                sh 'docker push steppenwol/zodiac:${BUILD_ID}'
            }
        }
    }
    
    post {
        failure{
            mail bcc: '', body: 'pas de chance', cc: '', from: '', replyTo: '', subject: 'Fail', to: 'admin@admin.com'
          }
        success{
            mail bcc: '', body: 'bravo', cc: '', from: '', replyTo: '', subject: 'Sucess', to: 'admin@admin.com'
           }
        }
}

