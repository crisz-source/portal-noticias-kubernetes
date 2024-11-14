# Deployment

#### Cada arquivo tem todas as configurações necessárias para a aplicação funcionar corretamente. Cada arquivo manifesto possui um deploy, configmag, service e futuramente os volumes.


```bash
# Deploy do frontend
kubectl apply -f ./portal-noticias/deploy/portal-noticias-deploy.yml

# Deploy do backend
kubectl apply -f ./portal-noticias/deploy/sistema-noticias-deploy.yml

# Deploy do banco
kubectl apply -f ./portal-noticias/deploy/db-noticias-deploy.yml

# Pegue o indereço ip: INTERNAL-IP do minikube com o comando [ kubectl get nodes -o wide ] e cole no navegador
internal-ip-minikube:30000  # exibe as noticias cadastrada 
internal-ip-minikube:30001 # tela de login (admin:admin)
```