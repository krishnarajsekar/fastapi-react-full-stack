pipeline {

    agent any

    tools {
        nodejs 'Node20'
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                credentialsId: 'github-token',
                url: 'https://github.com/krishnarajsekar/fastapi-react-full-stack.git'
            }
        }

        stage('Install Backend') {
            steps {
                sh '''
                cd backend

                python3 -m venv venv

                . venv/bin/activate

                pip install -r requirements.txt
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
                sudo rm -rf /var/www/html/*
                sudo cp -r frontend/dist/* /var/www/html/
                '''
            }
        }

        stage('Restart Backend') {
            steps {
                sh '''
                sudo systemctl restart fastapi
                '''
            }
        }
    }
}
