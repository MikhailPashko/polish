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
        stage('Make executable File') {
            agent { node { label 'node1'} }
            steps {
                sh '''
                docker cp /home/jenkins/workspace/Git-Pipeline-Pollish 404a848c957b:/home/jenkins/workspace
                docker exec -i 404a848c957b bash
                cd /home/jenkins/workspace/Git-Pipeline-Pollish/src
                make > ../Make.report
                exit 0
                docker cp 404a848c957b:/home/jenkins/workspace/Git-Pipeline-Pollish  /home/jenkins/workspace/Git-Pipeline-Pollish
                rsync -rvz /home/jenkins/workspace/Git-Pipeline-Pollish/ root@192.168.2.4:/var/lib/jenkins/workspace/Git-Pipeline-Pollish/
                '''
            }
        }
        stage('Formation general Report') {
            agent { node { label 'ubuntu_master'} }
            steps {
                sh '''
                cat CPPLint.report CPPCheck.report Make.report > General.report
                rm CPPLint.report CPPCheck.report Make.report
                ls -la
                '''
            }
        }
        stage('ZIP Executable file and General.Report') {
            agent { node { label 'ubuntu_master'} }
            steps {
                sh 'ls -la'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                echo 'Start stage'
                ls -la
                echo 'End stage'
                '''
            }
        }
    }    
    
}
