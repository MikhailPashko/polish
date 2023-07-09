pipeline {
    agent any
    environment {
        PROJECT_NAME = "Vostok"
        OWNER_NAME = "MIKHAIL PASHKO"
        DEPLOY_PACKAGE_NAME = "Application_executable_$GIT_COMMIT_$BUILD_ID.zip"
    }    
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
            agent { node { label 'ubuntu_master'} }
            steps {
                sh '''
                cd src
                make > ../Make.report
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
        stage('ZIP Executable file and General.Report and send to AppServer') {
            agent { node { label 'ubuntu_master'} }
            steps {
                sh ''' 
                zip -r $DEPLOY_PACKAGE_NAME ./ -i General.report build/graph
                '''
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
