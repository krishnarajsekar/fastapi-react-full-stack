pipeline {

    agent any

    tools {
        nodejs 'Node20'
    }

    stages {

        stage('Clone') {
            steps {
                git(
                    branch: 'main',
                    url: 'https://github.com/krishnarajsekar/fastapi-react-full-stack.git'
                )
            }
        }

        stage('Install Backend') {
            steps {
                sh '''
                cd backend

                python3 -m venv venv
                . venv/bin/activate

                pip install --upgrade pip
                pip install fastapi uvicorn

                pip install .
                '''
            }
        }

        stage('Build Frontend') {
            steps {
                sh '''
                cd frontend

                npm install
                npm run build
                '''
            }
        }

        stage('Deploy Frontend') {
            steps {
                sh '''
                sudo mkdir -p /var/www/html
                sudo rm -rf /var/www/html/*
                sudo cp -r frontend/dist/* /var/www/html/
                '''
            }
        }

        stage('Restart Backend') {
            steps {
                sh '''
                sudo systemctl restart fastapi || true
                '''
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}


