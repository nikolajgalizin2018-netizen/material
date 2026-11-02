pipeline {
    agent any
    
    environment {
        PATH = "/usr/local/go/bin:${env.PATH}"
        GOPATH = "${env.WORKSPACE}/go"
        GOROOT = "/usr/local/go"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo 'Код успешно получен из репозитория'
            }
        }
        
        stage('Verify Tools') {
            steps {
                sh '''
                    echo "=== Проверка версий инструментов ==="
                    go version
                    docker --version
                    echo "=== Текущая директория ==="
                    pwd
                    ls -la
                '''
            }
        }
        
        stage('Run Tests') {
            steps {
                sh '''
                    echo "=== Запуск модульных тестов ==="
                    go test ./... -v
                '''
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh '''
                    echo "=== Сборка Docker образа ==="
                    docker build -t myapp:${BUILD_NUMBER} .
                    echo "Образ успешно создан: myapp:${BUILD_NUMBER}"
                '''
            }
        }
        
        stage('Verify Docker Image') {
            steps {
                sh '''
                    echo "=== Проверка Docker образа ==="
                    docker images | grep myapp
                '''
            }
        }
    }
    
    post {
        success {
            echo 'Сборка успешно завершена!'
        }
        failure {
            echo 'Сборка завершилась с ошибкой!'
        }
    }
}
