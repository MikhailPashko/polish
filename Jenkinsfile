pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                  sudo apt install make
                  sudo apt install cppcheck
                  sudo apt install python3.9
                  cppcheck --version
                  python --version
                '''
            }
        } 
        
        stage('Test') {
            steps {
                sh '''
                ll
                '''
                echo 'Python Test'
                sh "python3 materials/linters/cpplint.py --extensions=c src/*.c"
                echo 'CPPCHECK Test'
                sh "cppcheck --enable=all --suppress=missingIncludeSystem src/"
                sh '''
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
