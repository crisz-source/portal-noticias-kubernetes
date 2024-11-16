## Aqui está os passo a passo para executar o projeto, para verificar o conteúdo do projeto portal de notícias no ubuntu 22.04, siga esses passos:

#### 1 - instale o docker
```bash
### # Atualize o gerenciador de pacotes
sudo apt update

# Instale pacotes necessários
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

# instalando o docker
sudo apt install docker.io docker-compose docker-compose-v2 docker-doc


# Inicie o Docker e adicione-o ao inicializar do sistema
sudo systemctl enable docker
sudo systemctl start docker

# adicionando docker a um grupo sem precisar ficar digitando "sudo" toda vez" 
sudo usermod -aG docker $USER
# Depois de disparar o comando acima, reinicie sua máquina ou saia da sessão e volte novamente para que o usermod funcione corretamente

#testando sem o sudo
docker ps
```

#### 2 - instalando o kubectl
```Bash
sudo apt-get install kubectl --classic
#ou
sudo snap install kubectl --classic
```


#### 3 - instale o minikube para uma criar um cluster local
```Bash
    curl -Lo minikube https://storage.googleapis.com/minikube/releases/v1.12.1/minikube-linux-amd64 \ && chmod +x minikube
    sudo install minikube /usr/local/bin/
```

#### 4 - Dê um start no minikube
```Bash
minikube start
```
#### 5 - Verifique o status 
```Bash
minikube status
```


# Aplicando os pods da aplicação separadamente, services e configmaps sem volumes

#### 1 - Entre no diretório portal-noticias-kubernetes e execute:

```Bash 
# frontend
kubectl apply -f ./portal-noticias/projeto/frontend/portal-noticias.yml # -- aplique apenas se NAO for utilizar o replicaset
kubectl apply -f ./portal-noticias/projeto/frontend/portal-configmap.yml
kubectl apply -f ./portal-noticias/projeto/frontend/svc-portal-noticias.yml


# backend
kubectl apply -f ./portal-noticias/projeto/backend/sistema-noticias.yml
kubectl apply -f ./portal-noticias/projeto/backend/sistema-configmap.yml
kubectl apply -f ./portal-noticias/projeto/backend/svc-sistema-noticias.yml


# Database 
kubectl apply -f ./portal-noticias/projeto/database-mysql/db-configmap.yml 
kubectl apply -f ./portal-noticias/projeto/database-mysql/db-noticias.yml
kubectl apply -f ./portal-noticias/projeto/database-mysql/svc-db-noticias.yml

# HPA (escalabilidade)
kubectl apply -f ./portal-noticias/projeto/frontend/portal-noticias-hpa.yml

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
O uso do escabilidade seria mais para garantir o funcionamento da aplicação, onde, quando um pod for deletado ele não é iniciado novamente e o replicaset muda este cenário, que em casos de falhas no pod o replicaset entra em ação e assim que um pod é terminado, automaticamente o replicaset inicia um outro pod. O deployment também faz parte da escalabilidade, um deployment é a mesma coisa de um replicaset, porém com uma camada acima... quando cria um deployment automaticamente esta definindo um replicaset

```bash
# caso queira testar o comando abaixo, delete o pod normal do portal-noticias e aplique o comando abaixo
kubectl apply -f ./portal-noticias/projeto/frontend/portal-noticias-replicasets.yml
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



















