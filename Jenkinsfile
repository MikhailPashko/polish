pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Build stage'
            }
        } 
        
        stage('Test cpplint') {
            agent { docker { image 'python:latest' } }
            steps {
                sh '''
                ls -la
                echo 'Python Test'
                "python3 materials/linters/cpplint.py --extensions=c src/*.c"
                ls -la
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Start stage'
                sh '''
                ls -la
                '''
                echo 'End stage'
            }
        }
    }    
    
}
