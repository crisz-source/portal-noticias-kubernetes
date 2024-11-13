## O projeto kubernetes, eu fiz feito em linux - ubuntu 22.04 e as imagens do projeto foi feita pela Alura. 

### Para verificar o conteúdo do projeto portal de notícias, siga esses passos:
##


#### 1 - instalando o kubectl
```Bash
sudo apt-get install kubectl --classic
#ou
sudo snap install kubectl --classic
```


#### 2 - instale o minikube para uma simulação de um cluster
```Bash
    curl -Lo minikube https://storage.googleapis.com/minikube/releases/v1.12.1/minikube-linux-amd64 \ && chmod +x minikube
    sudo install minikube /usr/local/bin/
```

#### 3 - Dê um start no minikube
```Bash
minikube start
```
#### 4 - Verifique o status 
```Bash
minikube status
```


# Aplicando os pods da aplicação separadamente, services e configmaps

#### 1 - Entre no diretório portal-notificias-main e execute:

```Bash 
# frontend

kubectl apply -f ./estudos-kubernetes-main/project-portal-noticias/project-portal-noticias-frontend/portal-noticias.yml
kubectl apply -f ./estudos-kubernetes-main/project-portal-noticias/project-portal-noticias-frontend/portal-configmap.yml

#opcional
kubectl apply -f ./estudos-kubernetes-main/project-portal-noticias/project-portal-noticias-frontend/portal-noticias-replicasets.yml


# backend
kubectl apply -f ./estudos-kubernetes-main/project-portal-noticias/project-sistema-noticias-backend/sistema-configmap.yml
kubectl apply -f ./estudos-kubernetes-main/project-portal-noticias/project-sistema-noticias-backend/sistema-noticias.yml
kubectl apply -f ./estudos-kubernetes-main/project-portal-noticias/project-sistema-noticias-backend/svc-sistema-noticias.yml


# Database 
kubectl apply -f ./estudos-kubernetes-main/project-portal-noticias/project-portal-db/db-configmap.yml 
kubectl apply -f ./estudos-kubernetes-main/project-portal-noticias/project-portal-db/db-noticias.yml
kubectl apply -f ./estudos-kubernetes-main/project-portal-noticias/project-portal-db/svc-db-noticias.yml

# entre na aplicação:
# usuário: admin
# senha: admin

# Entre na porta localhost:30001 (Windows)
# Para entrar na porta utilizando o ubuntu, execute o seguinte comando:
kubectl get node -o wide 

# Saída esperada:
NAME       STATUS   ROLES           AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION                       CONTAINER-RUNTIME
minikube   Ready    control-plane   16m   v1.31.0   192.168.49.2   <none>        Ubuntu 22.04.4 LTS   5.15.153.1-microsoft-standard-WSL2   docker://27.2.0


# caso não consiga entrar nessa porta, utilize o comando abaixo para que o minikube expõe uma URL e faz o um direcionamento da porta 30000 para uma aleatória
minikube service svc-portal-noticias


# Pegue o indereço ip: INTERNAL-IP e cole no navegador
192.168.49.2:30000 
192.168.49.2:30001

```

####
#
#
#
#


 # -- test Deploy --- em contrução

## Entre no diretório deploy e aplique o manifesto de deploy com todas as configurações necessárias:

```Bash
xxxxxx


```

#### x - Verifique se foi criado tudo corretamente  -- test
```Bash
kubectl get pods
kubectl get svc
kubectl get configmaps 
kubectl get pvc
kubectl get pv
kubectl get sc

```

# Volumes, entre no diretório de persistentVOlumeClaim e aplica os volumes -- test
```Bash
kubectl apply -f ./estudos-kubernetes-main/project-portal-noticias/volumes/persistentVolumeClaim/pod-sc.yml
kubectl apply -f ./estudos-kubernetes-main/project-portal-noticias/volumes/persistentVolumeClaim/pvc-sc.yml
```

## Entre no seu navegador com ip_interno_node:30000  -- test


