pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
        NVM_DIR = "${env.WORKSPACE}/.nvm"
    }

    stages {
        stage('Install Node.js and Yarn') {
            steps {
                sh '''
                    # Install nvm
                    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
                    
                    # Load nvm
                    export NVM_DIR="$HOME/.nvm"
                    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

                    # Install Node.js
                    nvm install 18
                    nvm use 18

                    # Install Yarn
                    npm install -g yarn
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
                    sh '''
                        # Load nvm
                        export NVM_DIR="$HOME/.nvm"
                        [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

                        # Use Node.js
                        nvm use 18
                        
                        yarn install
                    '''
                }
            }
        }
        stage('Add Env Vars to Config') {
            steps {
                dir('ftw-daily') {
                    sh '''
                        # Load nvm
                        export NVM_DIR="$HOME/.nvm"
                        [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

                        # Use Node.js
                        nvm use 18
                        
                        yarn run config
                    '''
                }
            }
        }
        stage('Start Dev Server') {
            steps {
                dir('ftw-daily') {
                    sh '''
                        # Load nvm
                        export NVM_DIR="$HOME/.nvm"
                        [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

                        # Use Node.js
                        nvm use 18
                        
                        yarn run dev
                    '''
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
