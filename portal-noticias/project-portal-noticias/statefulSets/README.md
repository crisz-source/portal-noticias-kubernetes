### O intuito deste StatefulSet seria mais para armazenar os dados da aplicação, como as imagens cadastradas no portal de notificas e a sessão.
#
### Para que funcione corretamente, será necessário executar o seguinte comando:


```bash
#apicando o persistentVolumeClaim das imagens
kubectl apply -f imagens-pvc.yml
```

```bash
#apicando o persistentVolumeClaim da sessao
kubectl apply -f sessao-pvc.yml
```

#
### Após isso, execute o comando para aplicar o statefulSet

```bash
kubectl apply -f sistema-noticias-statefulset-pvc.yml
```
#
### Abra no seu navegador na seguinte porta: localhost:30001 
#
### Caso estiver usando o minikube, será necessário realizar o seguinte comando para pegar o ip do node: 

```bash
kubectl get nodes -o wide

#pegue o INTERNAL-IP e adicione no seu navegador: ip:30001

```