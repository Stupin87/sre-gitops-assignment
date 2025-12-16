# sre-gitops-assignment
GitOps assignment with ArgoCD and k3s
## Описание проекта
Развертывание веб-приложения через GitOps с использованием:
- Kubernetes (k3s)
- ArgoCD
- Helm Charts
- Public GitHub репозиторий

### 1. Установка k3s
curl -sfL https://get.k3s.io | sh -sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config sudo chown $USER:$USER ~/.kube/config

### 2. Установка ArgoCD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl port-forward svc/argocd-server -n argocd 8080:443 &

### 4. Развертывание приложения

# Добавление репозитория в ArgoCD
argocd repo add https://github.com/YOUR_USERNAME/sre-gitops-assignment.git

# Создание Application
kubectl apply -f Ivanov_II/argocd/application.yaml

# Синхронизация
argocd app sync my-python-app

Проверка работы
Приложение доступно:
URL: http://<SERVER_IP>:30080

Health check: http://<SERVER_IP>:30080/

Статус в ArgoCD:
Состояние: Synced

Здоровье: Healthy

Ресурсы: 3 реплики

# Проверка приложения
curl http://localhost:30080

# Проверка ArgoCD
argocd app get my-python-app

# Проверка Kubernetes
kubectl get pods,svc,deploy


GitOps рабочий процесс
Изменение в Git: Редактируем values.yaml в GitHub

Автоматическая синхронизация: ArgoCD обнаруживает изменения

Развертывание: ArgoCD применяет изменения к кластеру

Проверка здоровья: ArgoCD проверяет статус развертывания


Масштабирование:
# values.yaml
replicaCount: 3  


Возникшие проблемы и решения
Проблема 1: ArgoCD не может получить доступ к репозиторию
Решение: Проверить URL репозитория и сетевую доступность

Проблема 2: Ошибка синхронизации
Решение:
argocd app sync my-python-app --prune
argocd app wait my-python-app

Проблема 3: Поды не запускаются
Решение: Проверить ресурсы и лимиты в values.yaml

##### Проверочный список для сдачи задания:

# 1. ArgoCD установлен

kubectl get pods -n argocd | grep Running

# 2. Приложение развернуто

argocd app get my-python-app | grep -E "(Name|Status|Health|Synced)"

# 3. Поды работают

kubectl get pods -l app=my-python-app

# 4. Сервис доступен

kubectl get svc my-python-app-service

# 5. Приложение отвечает

curl -s http://$(hostname -I | awk '{print $1}'):30080 | head -2

# 6. GitHub репозиторий

argocd repo list | grep github.com



  

