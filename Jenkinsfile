pipeline {
    agent   {
                label 'srv01dev-anindra'
            }
    
    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/devopsarn/simple-apps.git'
            }
        }
        stage('Build') {
            steps {
                sh '''cd apps
                npm install'''
            }
        }
        stage('Testing') {
            steps {
                sh '''cd apps
                npm test
                npm run test:coverage'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''cd apps
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.14.101:9000 \
                -Dsonar.login=sqp_92e2d08b97a6ac5eeddc442ea869bf726da56ad1'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }
    }
}