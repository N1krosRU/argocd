# argocd

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

Деплоим сущности Application которые будет следить за нашими манифестами
```bash
kubectl apply -f kind-test-cluster/applications/hello-world.yaml
kubectl apply -f kind-test-cluster/applications/bye-world.yaml
```

Вариант Application of Applications, здесь мы деплоим корневой Application, который задеплоит все наши Applications
```bash
kubectl apply -f kind-test-cluster/root.yaml
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
