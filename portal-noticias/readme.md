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


# Aplicando os pods da aplicação separadamente, services e configmaps sem volumes

#### 1 - Entre no diretório portal-notificias-main e execute:

```Bash 
# frontend
kubectl apply -f ./portal-noticias/project-portal-noticias/project-portal-noticias-frontend/portal-noticias.yml # -- aplique apenas se nao for utilizar o replicaset
kubectl apply -f ./portal-noticias/project-portal-noticias/project-portal-noticias-frontend/portal-configmap.yml
kubectl apply -f ./portal-noticias/project-portal-noticias/project-portal-noticias-frontend/svc-portal-noticias.yml


# backend
kubectl apply -f ./portal-noticias/project-portal-noticias/project-sistema-noticias-backend/sistema-noticias.yml
kubectl apply -f ./portal-noticias/project-portal-noticias/project-sistema-noticias-backend/sistema-configmap.yml
kubectl apply -f ./portal-noticias/project-portal-noticias/project-sistema-noticias-backend/svc-sistema-noticias.yml


# Database 
kubectl apply -f ./portal-noticias/project-portal-noticias/project-portal-db/db-configmap.yml 
kubectl apply -f ./portal-noticias/project-portal-noticias/project-portal-db/db-noticias.yml
kubectl apply -f ./portal-noticias/project-portal-noticias/project-portal-db/svc-db-noticias.yml

# entre na aplicação:
# usuário: admin
# senha: admin

# Entre na porta localhost:30001 (Windows)
# Para entrar na porta utilizando o ubuntu, execute o seguinte comando:
kubectl get node -o wide 

# Saída esperada:
NAME       STATUS   ROLES           AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION                       CONTAINER-RUNTIME
minikube   Ready    control-plane   16m   v1.31.0   192.168.49.2   <none>        Ubuntu 22.04.4 LTS   5.15.153.1-microsoft-standard-WSL2   docker://27.2.0


# Pegue o indereço ip: INTERNAL-IP e cole no navegador
internal-ip-minikube:30000 
internal-ip-minikube:30001

# caso não consiga entrar nessa porta, utilize o comando abaixo para que o minikube expõe uma URL e faz o um direcionamento da porta 30000 para uma aleatória
minikube service svc-portal-noticias
```
# Escalabilidade    
#### O uso do escabilidade seria mais para garantir o funcionamento da aplicação, onde, quando um pod for deletado ele não é iniciado novamente e o replicaset muda este cenário, que em casos de falhas no pod o replicaset entra em ação e assim que um pod é terminado, automaticamente o replicaset inicia um outro pod. O deployment também faz parte da escalabilidade, um deployment é a mesma coisa de um replicaset, porém com uma camada acima... quando cria um deployment automaticamente esta definindo um replicaset

```bash
# caso queira testar o comando abaixo, delete o pod normal do portal-noticias e aplique o comando abaixo
kubectl apply -f ./portal-noticias/project-portal-noticias/project-portal-noticias-frontend/portal-noticias-replicasets.yml
```

#### Verifique se foi criado tudo corretamente  -- test
```Bash
kubectl get pods
kubectl get svc
kubectl get configmaps 
kubectl get pvc
kubectl get pv
kubectl get sc
```



















