# Справочник по Kubernetes

## Расшифровка названий
* **Kubernetes** — от греч. «кормчий» (рулевой). Сокращение: K8s (8 букв между K и s).
* **kubectl** — *Kube Control* (Управление Kube). Утилита командной строки.
* **Minikube** — *Mini Kube* (Мини-куб). Локальный кластер Kubernetes.
* **Pod** — *Pod* (Стручок). Минимальная единица развертывания.
* **Deployment** — *Deployment* (Развертывание). Управляет подами.
* **Service** — *Service* (Служба). Стабильная точка доступа.

## Основные команды
* `minikube start` — запустить локальный кластер.
* `minikube stop` — остановить кластер.
* `minikube ip` — узнать IP-адрес кластера.
* `kubectl get nodes` — список нод.
* `kubectl get pods` — список подов.
* `kubectl get deployments` — список деплойментов.
* `kubectl get services` — список сервисов.
* `kubectl apply -f deployment.yaml` — применить манифест.
* `kubectl delete -f deployment.yaml` — удалить ресурсы.
* `kubectl logs <pod_name>` — посмотреть логи пода.
* `kubectl describe pod <pod_name>` — подробная информация о поде.