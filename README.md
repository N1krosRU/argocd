# argocd

Поднимаем argocd в домашних условиях
```bash
# Поднимите kubernetes кластер, можно использовать kind или любой другой который вам не жалко

# Создаём неймспейс под ArgoCD и разворачиваем его
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Прокидываем порт веб сервера ArgoCD на localhost:8443
kubectl port-forward -n argocd service/argocd-server 8443:443

# Получаем пароль от вебморды
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo

# Авторизуемся в ArgoCD по ссылке https://localhost:8443/
# username: admin
# password: из команды выше

# Создаём подключение к репозиторию: https://localhost:8443/settings/repos
# method: VIA HTTPS
# Type: git
# Project: default
# Repository URL: https://github.com/N1krosRU/argocd.git

# Клонируем репозиторий с проектом ArgoCD / форкните чтобы был свой
git clone https://github.com/N1krosRU/argocd
cd argocd

# Деплоим сущности Application которые будет следить за нашими манифестами
kubectl apply -f kind-test-cluster/applications/hello-world.yaml
kubectl apply -f kind-test-cluster/applications/bye-world.yaml

# Вариант Application of Applications, здесь мы деплоим корневой Application, который задеплоит все наши Applications
kubectl apply -f kind-test-cluster/root.yaml
```

Структура репозитория
```bash
├── kind-test-cluster               # Cluster name
│   ├── applications                # Application Dir
│   │   ├── bye-world.yaml
│   │   └── hello-world.yaml
│   ├── projects                    # Project Manifest Dir
│   │   ├── bye-world
│   │   │   └── deployment.yaml
│   │   └── hello-world
│   │       └── deployment.yaml
│   └── root.yaml                   # Root ArgoCD Application
└── README.md
```

Пример более сложной структуры репозитория
```bash
│
├── HelmCharts                 # All Helm Charts
│   ├── ChartTest1
│   │   ├── Chart.yaml
│   │   ├── templates
│   │   ├── values_dev.yaml    # DEV Values
│   │   ├── values_prod.yaml   # PROD Values
│   │   └── values.yaml        # Default Values
│   └── ChartTest2
│       ├── Chart.yaml
│       ├── templates
│       ├── values_dev.yaml    # DEV Values
│       ├── values_prod.yaml   # PROD Values
│       └── values.yaml        # Default Values
│   
├── сluster-dev                # Cluster name
│   ├── applications
│   │   ├── app1.yaml
│   │   └── app2.yaml
│   └── root.yaml              # Root ArgoCD Application
└── сluster-prod               # Cluster name
    ├── applications
    │   ├── app1.yaml
    │   └── app2.yaml
    └── root.yaml              # Root ArgoCD Application    
```
