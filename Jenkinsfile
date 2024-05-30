pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
    }

    stages {
        stage('Install Node.js and Yarn') {
            steps {
                sh '''
                    # Install Node.js
                    curl -fsSL https://deb.nodesource.com/setup_14.x | bash -
                    apt-get install -y nodejs
                    
                    # Install Yarn
                    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
                    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
                    apt-get update && apt-get install -y yarn
                '''
            }
        }
        stage('Checkout') {
            steps {
                git 'git@github.com:sharetribe/ftw-daily.git'
            }
        }
        stage('Change Directory') {
            steps {
                dir('ftw-daily') {
                    script {
                        env.WORKDIR = pwd()
                    }
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                dir('ftw-daily') {
                    sh 'yarn install'
                }
            }
        }
        stage('Add Env Vars to Config') {
            steps {
                dir('ftw-daily') {
                    sh 'yarn run config'
                }
            }
        }
        stage('Start Dev Server') {
            steps {
                dir('ftw-daily') {
                    sh 'yarn run dev'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace'
            cleanWs()
        }
        success {
            echo 'Build completed successfully'
        }
        failure {
            echo 'Build failed'
        }
    }
}
