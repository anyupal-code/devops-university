# Урок 11. Введение в Kubernetes (K8s)

## Цель урока
Понять архитектуру Kubernetes, освоить основные объекты (Pod, Deployment, Service) и научиться запускать приложение в локальном кластере (Minikube).

## Что потребуется для этого урока
* **Docker** (установленный и запущенный).
* **Minikube** (локальный кластер Kubernetes для обучения).
* **kubectl** (утилита командной строки для управления кластером).

---

## Теория

### 1. Зачем нужен Kubernetes?
Docker Compose отлично работает на одном сервере. Но если сервер упадет или нагрузка вырастет в 10 раз, Docker Compose не сможет автоматически запустить приложение на другом сервере. 
Kubernetes — это оркестратор, который управляет кластером из множества серверов (нод), автоматически распределяет нагрузку, перезапускает упавшие контейнеры и масштабирует приложение.

### 2. Основные объекты
* **Pod** — минимальная единица. Содержит один или несколько контейнеров (обычно один). Поды эфемерны: если под упал, создается новый.
* **Deployment** — управляет подами. Обеспечивает заданное количество реплик (например, 3 пода) и автоматически перезапускает их. Позволяет обновлять приложение без простоя.
* **Service** — стабильная точка доступа к подам. Поскольку IP-адреса подов меняются, Service предоставляет единый IP и балансирует нагрузку.

### 3. Установка Minikube
Minikube создает локальный кластер Kubernetes (обычно на одной виртуальной машине или в Docker-контейнере).

Установка в Linux:
~~~bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
~~~

Запуск кластера:
~~~bash
minikube start --driver=docker
~~~

### 4. Утилита kubectl
`kubectl` — это основной инструмент для работы с Kubernetes. Установка:
~~~bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
~~~

Проверка:
~~~bash
kubectl get nodes
~~~
*(Должна появиться одна нода в статусе Ready).*

### 5. Манифесты (YAML)
В Kubernetes вы не пишете команды «запусти контейнер». Вы описываете **желаемое состояние** в YAML-файле (манифесте). Kubernetes сам приводит систему к этому состоянию.

Пример манифеста для Deployment:
~~~yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: backend-app
  template:
    metadata:
      labels:
        app: backend-app
    spec:
      containers:
      - name: backend-app
        image: your-username/backend-app:latest
        ports:
        - containerPort: 8080
~~~

* `replicas: 2` — Kubernetes запустит 2 пода с приложением.
* `image` — образ из Docker Hub (или локальный).

### 6. Применение манифеста
~~~bash
kubectl apply -f deployment.yaml
~~~
После этого Kubernetes создаст поды. Проверить их статус:
~~~bash
kubectl get pods
~~~

---

## Практика
Переходите к [Справочнику команд](COMMANDS.md) и [Заданиям](TASKS.md).

---

## Чек-лист
- [ ] Понимаю, зачем нужен Kubernetes.
- [ ] Знаю основные объекты (Pod, Deployment, Service).
- [ ] Умею устанавливать Minikube и kubectl.
- [ ] Умею писать манифест для Deployment.
- [ ] Умею применять манифест через `kubectl apply`.

---

## Что дальше
На следующем уроке мы соберем **Итоговый проект**: полный цикл CI/CD с деплоем в Docker.