# Задания к Уроку 11

## Задание 1. Установка Minikube и kubectl
1. Установите Minikube и kubectl (команды в теории).
2. Запустите кластер: `minikube start --driver=docker`.
3. Проверьте статус: `kubectl get nodes`.
**Ожидаемый результат:** нода в статусе `Ready`.

## Задание 2. Создание Deployment
1. Создайте файл `deployment.yaml` с манифестом из теории (замените `your-username/backend-app:latest` на ваш образ из Docker Hub).
2. Примените манифест: `kubectl apply -f deployment.yaml`.
3. Проверьте, что поды запустились: `kubectl get pods`.
**Ожидаемый результат:** два пода в статусе `Running`.

## Задание 3. Создание Service
1. Создайте файл `service.yaml`:
   ~~~yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: backend-service
   spec:
     type: NodePort
     selector:
       app: backend-app
     ports:
       - port: 8080
         targetPort: 8080
         nodePort: 30080
   ~~~
2. Примените: `kubectl apply -f service.yaml`.
3. Узнайте IP-адрес Minikube: `minikube ip`.
4. Откройте в браузере: `http://<IP-адрес>:30080`.

**Ожидаемый результат:** ваше Java-приложение доступно по указанному адресу.

## Задание 4 (*). Масштабирование
Увеличьте количество реплик до 4:
~~~bash
kubectl scale deployment backend-app --replicas=4
~~~
Проверьте, что создались новые поды: `kubectl get pods`.