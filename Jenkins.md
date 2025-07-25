# Домашнее задание к занятию "DevOps. CI/CD" - `Хамуро Илья`

---

### Задание 1
Выполнил задание с использованием docker, без vagrant, делал работу на вм в Яндекс облаке

![Снимок экрана 2025-05-15 234540](https://github.com/user-attachments/assets/321af4b4-eeef-45bb-9da4-2714b3068b87)
![Снимок экрана 2025-05-15 234258](https://github.com/user-attachments/assets/d562a653-a616-4760-b853-65bffc412352)
![Снимок экрана 2025-05-15 234226](https://github.com/user-attachments/assets/26168e79-80b8-4fa7-823d-fe25140ab640)
![Снимок экрана 2025-05-15 234231](https://github.com/user-attachments/assets/c32f86c4-9011-4dce-bc5c-81f9eab62246)
![Снимок экрана 2025-05-15 234236](https://github.com/user-attachments/assets/6d1287e5-d2f4-4291-93e0-467143adc809)



---

### Задание 2

pipeline {
    agent any

    environment {
        REPO_URL = "https://github.com/IlyaKham/sdvps-materials.git"
        REGISTRY = "158.160.23.101:8082"
        IMAGE = "${REGISTRY}/hello-world"
        VERSION = "v${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Клонируем репозиторий...'
                git url: "${REPO_URL}", branch: 'main'
            }
        }

        stage('Go Test') {
            steps {
                echo '🧪 Запускаем go test...'
                sh '/usr/bin/go test .'
            }
        }

        stage('Docker Build') {
            steps {
                echo '🐳 Собираем Docker-образ...'
                sh 'docker build -t ${IMAGE}:${VERSION} .'
            }
        }

        stage('Docker Push') {
            steps {
                echo '📦 Публикуем в Nexus...'
                sh '''
                    docker login ${REGISTRY} -u admin -p zaq123
                    docker push ${IMAGE}:${VERSION}
                    docker logout ${REGISTRY}
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Успешно загружено в Nexus: ${IMAGE}:${VERSION}"
        }
        failure {
            echo "❌ Ошибка в пайплайне."
        }
    }
}

![Снимок экрана 2025-05-16 000653](https://github.com/user-attachments/assets/e188e4d0-fae0-4592-bd4e-0cae72e9f2c2)



