pipeline {
    agent any
    environment {
        DEPLOY_PACKAGE_NAME = "Application_executable_${GIT_COMMIT}_${BUILD_NUMBER}.zip"
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
        stage('Deploy To Server') {
            agent { node { label 'ubuntu_master'} }
            steps {
                sh ''' 
                scp $DEPLOY_PACKAGE_NAME root@192.168.2.11:/home
                '''
            }
        }
    } 
        post {
        always{
                archiveArtifacts artifacts: '*.zip', onlyIfSuccessful: true
                
                emailext to: "michaelpipinn@gmail.com",
                subject: "jenkins build:${currentBuild.currentResult}: ${env.JOB_NAME}",
                body: "${currentBuild.currentResult}: Job ${env.JOB_NAME}\nMore Info can be found here: ${env.BUILD_URL}",
                attachmentsPattern: '*.zip'
                
        cleanWs()
            }
        }
}
