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
                echo 'Python Test'
                python3 materials/linters/cpplint.py --extensions=c src/*.c > CPPLint.report
                '''
            }
        }
        stage('Test cppcheck') {
            agent { node { label 'ubuntu_master'} }
            steps {
                sh '''
                ls -la
                echo 'Python Test'
                cppcheck --enable=all --suppress=missingIncludeSystem src/ > CPPCheck.report
                ls -la
                '''
            }
        }
        stage('Formation general Report') {
            agent { node { label 'ubuntu_master'} }
            steps {
                sh '''
                cat CPPLint.report CPPCheck.report > General.report
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
