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
                ll
                echo 'Python Test'
                "python3 materials/linters/cpplint.py --extensions=c src/*.c"
                ll
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Start stage'
                sh '''
                ll
                '''
                echo 'End stage'
            }
        }
    }    
    
}
