# O StatefulSet é usado principalmente para aplicações que requerem identidade estável e armazenamento persistente. Ele é ideal para situações em que os pods precisam manter um identificador único e persistente, como o caso de bancos de dados ou sistemas que armazenam dados locais, como imagens ou sessões, que não podem ser simplesmente descartados e recriados com facilidade.

### Para que funcione corretamente, será necessário executar o seguinte comando:


```bash
#apicando o persistentVolumeClaim que é o armazenamento das imagens
kubectl apply -f ./portal-noticias/project-portal-noticias/statefulSets/imagens-pvc.yml
```

```bash
#apicando o persistentVolumeClaim que é o armazenamento da sessao
kubectl apply -f ./portal-noticias/project-portal-noticias/statefulSets/sessao-pvc.yml
```

#
### Após isso, execute o comando para aplicar o statefulSet

```bash
kubectl apply -f ./portal-noticias/project-portal-noticias/statefulSets/sistema-noticias-statefulset.yml
```
#
### Abra no seu navegador na seguinte porta: localhost:30001 
#
### Caso estiver usando o minikube, será necessário realizar o seguinte comando para pegar o ip do node: 

```bash
kubectl get nodes -o wide

#pegue o INTERNAL-IP e adicione no seu navegador: internal-ip:30001
# cadastre noticias
# depois de cadastrar uma noticia, entre na porta 30000 aperte o F5 e verifique se os dados ainda persistiu, delete o pod e entre novamente na porta 30000 e veja se as noticias cadastrada no antigo pod ainda persiste:
kubectl delete pod

```