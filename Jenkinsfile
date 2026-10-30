
pipeline {
    agent any

    options {
        timestamps()
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Getting Laravel project from GitHub'
                checkout scm
            }
        }

        stage('Prepare Environment') {
            steps {
                sh '''
                    cp .env.example .env

                    APP_KEY="base64:$(openssl rand -base64 32 | tr -d '\\n')"

                    sed -i "s|^APP_KEY=.*|APP_KEY=${APP_KEY}|" .env
                    sed -i 's|^APP_ENV=.*|APP_ENV=production|' .env
                    sed -i 's|^APP_DEBUG=.*|APP_DEBUG=false|' .env
                    sed -i 's|^APP_URL=.*|APP_URL=http://localhost:8081|' .env

                    sed -i 's|^DB_CONNECTION=.*|DB_CONNECTION=mysql|' .env
                    sed -i 's|^DB_HOST=.*|DB_HOST=db|' .env
                    sed -i 's|^DB_PORT=.*|DB_PORT=3306|' .env
                    sed -i 's|^DB_DATABASE=.*|DB_DATABASE=laravel_db|' .env
                    sed -i 's|^DB_USERNAME=.*|DB_USERNAME=laravel_user|' .env
                    sed -i 's|^DB_PASSWORD=.*|DB_PASSWORD=LaravelPass123|' .env
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Deploy Application') {
            steps {
                sh 'docker compose up -d'
            }
        }

        stage('Run Database Migrations') {
            steps {
                sh 'docker compose exec -T app php artisan migrate --force'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'docker compose ps'
                echo 'Laravel deployment completed successfully!'
            }
        }
    }
}
