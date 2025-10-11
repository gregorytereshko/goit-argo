# GitOps з ArgoCD та Terraform

## Як запустити Terraform

cd terraform/vpc && terraform init -reconfigure && terraform apply -auto-approve  
cd ../eks && terraform init -reconfigure && terraform apply -auto-approve  
cd ../argocd && terraform init -reconfigure && terraform apply -auto-approve  

ArgoCD буде встановлено у кластер у namespace infra-tools.

## Як перевірити, що ArgoCD працює

kubectl get pods -n infra-tools  

Очікувані pod-и:  
argocd-server  
argocd-repo-server  
argocd-application-controller  
argocd-applicationset-controller  
argocd-redis  

Усі мають бути в стані Running.

## Як відкрити UI ArgoCD

Оскільки сервіс типу ClusterIP, доступ виконується через port-forward:  

kubectl port-forward svc/argocd-server -n infra-tools 8080:443  

Відкрити у браузері: https://localhost:8080  

Пароль адміністратора:  
kubectl -n infra-tools get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode  

## Як перевірити, що деплой відбувся

ArgoCD автоматично синхронізується з репозиторієм goit-argo і створює Applications для кожного namespace.  

kubectl -n infra-tools get applications  
kubectl get pods -n application  
kubectl get pods -n development  

Усі Applications мають бути в статусі Healthy / Synced.

## Посилання на репозиторій

https://github.com/gregorytereshko/goit-argo  

goit-argo/  
├── namespaces/  
│   ├── application/  
│   │   ├── ns.yaml  
│   │   └── configmaps/common-cm.yaml
│   ├── development/  
│   │   └── ns.yaml
│   │   └── apps/demo-nginx.yaml  
│   └── infra-tools/  
│       └── ns.yaml  
└── application.yaml  