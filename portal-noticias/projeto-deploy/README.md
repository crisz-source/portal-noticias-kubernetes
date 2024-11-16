# Deployment

#### Cada arquivo tem todas as configurações necessárias para a aplicação funcionar corretamente. Cada arquivo manifesto possui um deploy, configmag, service e futuramente os volumes.


```bash
# Deploy e HPA do frontend
kubectl apply -f ./portal-noticias/projeto-deploy/portal-noticias-deploy/portal-noticias-deploy.yml
kubectl apply -f ./portal-noticias/projeto-deploy/portal-noticias-deploy/portal-noticias-hpa.yml

# Deploy do backend
kubectl apply -f ./portal-noticias/projeto-deploy/sistema-noticias-deploy.yml

# Deploy do banco
kubectl apply -f ./portal-noticias/projeto-deploy/db-noticias-deploy.yml

# Pegue o indereço ip: INTERNAL-IP do minikube com o comando [ kubectl get nodes -o wide ] e cole no navegador
internal-ip-minikube:30000  # exibe as noticias cadastrada 
internal-ip-minikube:30001 # tela de login (admin:admin)

```

# livenessProbe

A livenessProbe é usada para verificar se o container está vivo e se o processo dentro dele ainda está funcionando corretamente. Se o container não responder corretamente à livenessProbe (indicando falha), o Kubernetes vai reiniciar o container.

- **Intervalo de Verificação:** definir o intervalo em que a verificação neste caso do manifesto *portal-noticias-deploy.yml* a cada 10 segundos.

- **Número de Tentativas:** pode definir quantas tentativas de falha serão feitas antes de reiniciar o container. No manifesto *portal-noticias-deploy.yml*, configurei para que o Kubernetes tente 3 vezes antes de decidir reiniciar o container.

- **Atraso Inicial:** O *initialDelaySeconds* é o tempo de espera após o início do container até a primeira verificação da livenessProbe.

- **Códigos de Sucesso e Falha:** A livenessProbe considera um código HTTP no intervalo de 200-399 como sucesso. Qualquer código fora desse intervalo, como 400-599, será considerado uma falha.


# readinessProbe

Os readniessProbe, a única diferente dele entre livenessProbe é que o readness espera um container funcionar para entrar em ação, ou seja, sua função principal é veriricar se o container está pronto para receber os tráfegos. Ela só entra em ação após o cintainr estar em funcionamento, após o processo de inicialização


# Diferença principal entre livenessProbe e readinessProbe:

- **LivenessProbe:** Verifica se o container está vivo e funcionando. Se falhar, o container será reiniciado.

- **ReadinessProbe:** Verifica se o container está pronto para receber tráfego. Se falhar, o tráfego não será enviado para ele, mas o container não será reiniciado.


# HPA (Horizontal Pod Autoscaler)

Este recurso é autoescalável, por exemplo, se uma aplicação que possui um serviço que esta "vinculad" a um pod e essa aplicação possui muitas requisição ao mesmo tempo, a tendência é que o pod chegue ao seu limite de CPU e acabe sendo desligado por conta de muitas requisições, o HPA esolve isso de forma automática se uma aplicação possuir muitas requisições de uma vez só. 

- O HPA monitora as métricas de utilização dos pods (como CPU e memória por default) e, se a utilização dessas métricas ultrapassar um limite configurado, o HPA automaticamente escala o número de pods para atender a demanda.

- Se a utilização de CPU ou memória de um pod atingir um valor que indique que ele está sobrecarregado, o HPA pode criar novos pods para distribuir a carga, evitando que a aplicação sofra quedas ou lentidão.

- Quando a carga diminui, o HPA pode reduzir o número de pods para economizar recursos.


# Configuração do HPA

# servidor de metricas
```bash
# liste os addons do minikube
minikube addons list

# saída esperada: 
|-----------------------------|----------|--------------|--------------------------------|
|         ADDON NAME          | PROFILE  |    STATUS    |           MAINTAINER           |
|-----------------------------|----------|--------------|--------------------------------|
| ambassador                  | minikube | disabled     | 3rd party (Ambassador)         |
| auto-pause                  | minikube | disabled     | minikube                       |
| cloud-spanner               | minikube | disabled     | Google                         |
| csi-hostpath-driver         | minikube | disabled     | Kubernetes                     |
| dashboard                   | minikube | disabled     | Kubernetes                     |
| default-storageclass        | minikube | enabled ✅   | Kubernetes                     |
| efk                         | minikube | disabled     | 3rd party (Elastic)            |
| freshpod                    | minikube | disabled     | Google                         |
| gcp-auth                    | minikube | disabled     | Google                         |
| gvisor                      | minikube | disabled     | minikube                       |
| headlamp                    | minikube | disabled     | 3rd party (kinvolk.io)         |
| helm-tiller                 | minikube | disabled     | 3rd party (Helm)               |
| inaccel                     | minikube | disabled     | 3rd party (InAccel             |
|                             |          |              | [info@inaccel.com])            |
| ingress                     | minikube | disabled     | Kubernetes                     |
| ingress-dns                 | minikube | disabled     | minikube                       |
| inspektor-gadget            | minikube | disabled     | 3rd party                      |
|                             |          |              | (inspektor-gadget.io)          |
| istio                       | minikube | disabled     | 3rd party (Istio)              |
| istio-provisioner           | minikube | disabled     | 3rd party (Istio)              |
| kong                        | minikube | disabled     | 3rd party (Kong HQ)            |
| kubeflow                    | minikube | disabled     | 3rd party                      |
| kubevirt                    | minikube | disabled     | 3rd party (KubeVirt)           |
| logviewer                   | minikube | disabled     | 3rd party (unknown)            |
| metallb                     | minikube | disabled     | 3rd party (MetalLB)            |
| metrics-server              | minikube | disabled     | Kubernetes                     |
| nvidia-device-plugin        | minikube | disabled     | 3rd party (NVIDIA)             |
| nvidia-driver-installer     | minikube | disabled     | 3rd party (NVIDIA)             |
| nvidia-gpu-device-plugin    | minikube | disabled     | 3rd party (NVIDIA)             |
| olm                         | minikube | disabled     | 3rd party (Operator Framework) |
| pod-security-policy         | minikube | disabled     | 3rd party (unknown)            |
| portainer                   | minikube | disabled     | 3rd party (Portainer.io)       |
| registry                    | minikube | disabled     | minikube                       |
| registry-aliases            | minikube | disabled     | 3rd party (unknown)            |
| registry-creds              | minikube | disabled     | 3rd party (UPMC Enterprises)   |
| storage-provisioner         | minikube | enabled ✅   | minikube                       |
| storage-provisioner-gluster | minikube | disabled     | 3rd party (Gluster)            |
| storage-provisioner-rancher | minikube | disabled     | 3rd party (Rancher)            |
| volcano                     | minikube | disabled     | third-party (volcano)          |
| volumesnapshots             | minikube | disabled     | Kubernetes                     |
| yakd                        | minikube | disabled     | 3rd party (marcnuri.com)       |
|-----------------------------|----------|--------------|--------------------------------|

# adicione o metrics-server
minikube addons enable metrics-server

# saída esperada após abilitar o metrics-server
💡  metrics-server is an addon maintained by Kubernetes. For any concerns contact minikube on GitHub.
You can view the list of minikube maintainers at: https://github.com/kubernetes/minikube/blob/master/OWNERS
    ▪ Using image registry.k8s.io/metrics-server/metrics-server:v0.7.2
🌟  The 'metrics-server' addon is enabled

# verifique o hpa e o target
kubectl get hpa

# saída esperada
NAME                  REFERENCE                               TARGETS              MINPODS   MAXPODS   REPLICAS   AGE
portal-noticias-hpa   Deployment/portal-noticias-deployment   cpu: <unknown>/50%   1         10        3          8m30s

# caso o target não mude, aguarde alguns minutinhos que vai funcionar
# Depois de uns 20 segundos, tive o seguinte resultado:
# monitore em tempo real com o comando --watch
kubectl get hpa --watch
NAME                  REFERENCE                               TARGETS        MINPODS   MAXPODS   REPLICAS   AGE
portal-noticias-hpa   Deployment/portal-noticias-deployment   cpu: 10%/50%   1         10        1          10m

# caso a cpu não chegue ao limite, eu criei um shell script para realizar um teste estresse: 

#!/bin/bash
for i in {1..10000}; do
    curl http://192.168.49.2:30000
    sleep $1
done

#execute o script:
bash ./portal-noticias/deploy/portal-noticias-deploy/teste-stress.sh 0.001 > out.txt &

# saída esperada do script:
100  5934  100  5934    0     0  4410k      0 --:--:-- --:--:-- --:--:-- 5794k
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5934  100  5934    0     0  3974k      0 --:--:-- --:--:-- --:--:-- 5794k
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5934  100  5934    0     0  5141k      0 --:--:-- --:--:-- --:--:-- 5794k
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5934  100  5934    0     0  4906k      0 --:--:-- --:--:-- --:--:-- 5794k
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5934  100  5934    0     0  4321k      0 --:--:-- --:--:-- --:--:-- 5794k
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
....

# Com o script rodando, abra um outro terminal e execute o comando para monitorar o hpa em ação
kubectl get hpa --watch

# saída esperada:
NAME                  REFERENCE                               TARGETS        MINPODS   MAXPODS   REPLICAS   AGE
portal-noticias-hpa   Deployment/portal-noticias-deployment   cpu: 10%/50%   1         10        1          20m
portal-noticias-hpa   Deployment/portal-noticias-deployment   cpu: 390%/50%   1         10        1          20m
portal-noticias-hpa   Deployment/portal-noticias-deployment   cpu: 390%/50%   1         10        4          20m
portal-noticias-hpa   Deployment/portal-noticias-deployment   cpu: 390%/50%   1         10        8          20m
portal-noticias-hpa   Deployment/portal-noticias-deployment   cpu: 360%/50%   1         10        8          21m

# Note que enquanto o shell script esta rodando, as replicas são criadas, agora, cancele o shell script com CTRL+C e monitore novamente e verá que as replicas vai diminuindo até chegar 1 replica
# saída esperada após o cancelamento do script
NAME                  REFERENCE                               TARGETS        MINPODS   MAXPODS   REPLICAS   AGE
portal-noticias-hpa   Deployment/portal-noticias-deployment   cpu: 81%/50%   1         10        10         26m
portal-noticias-hpa   Deployment/portal-noticias-deployment   cpu: 78%/50%   1         10        10         27m
portal-noticias-hpa   Deployment/portal-noticias-deployment   cpu: 10%/50%   1         10        10         28mNAME                  
portal-noticias-hpa   Deployment/portal-noticias-deployment   cpu: 10%/50%   1         10        1         31m
```

# => OBS: O shell script acima, será necessário adicionar o ip do cluster minikube, para verifica o ip, digite:
```bash
kubectl get nodes -o wide

#saída esperada: ( o ip fica em INTERNAL-IP)
NAME       STATUS   ROLES           AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
minikube   Ready    control-plane   34h   v1.31.0   192.168.49.2   <none>        Ubuntu 22.04.4 LTS   6.8.0-48-generic   docker://27.2.0

```