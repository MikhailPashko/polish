pipeline {
    agent { node { label 'ubuntu_master'}}
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
            agent { node { label 'ubuntu20'} }
            steps {
                sh '''
                ls -la
                echo 'Python Test'
                cppcheck --enable=all --suppress=missingIncludeSystem src/ > CPPCheck.report
                ls -la
                '''
            }
        }
        stage('Make executable File') {
            agent { docker { image 'gcc:latest'} }
            steps {
                sh '''
                ls -la
                cd src/
                make > ../Make.report
                '''
            }
        }
        stage('Formation general Report') {
            agent { node { label 'ubuntu20'} }
            steps {
                sh '''
                cat CPPLint.report CPPCheck.report Make.report > General.report
                rm CPPLint.report CPPCheck.report Make.report
                ls -la
                '''
            }
        }
        stage('ZIP Executable file and General.Report') {
            agent { node { label 'ubuntu20'} }
            steps {
                sh 'ls -la'
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
