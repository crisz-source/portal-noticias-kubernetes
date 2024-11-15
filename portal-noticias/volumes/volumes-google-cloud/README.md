# Este diretório foi mais para realização de teste na google cloud, para ver o funcionamento dos pv, pvc e storage class da propria google cloud. 

#### Os arquivos estão "contectados" da seguinte forma

```bash
# pod-pv.yml => é um pod simples que esta usando o PVC pvc-1 
# pv.yml => Definição de um persistentVolume da google cloud,
# pvc.yml => Definição de um persistentVolumeClaim

# Basícamente, este cenário está tendo um pod que é um contêiner que vai usar um armazenamento. Nesse caso, ele depende do PVC para solicitar um volume persistente de armazenamento. 

#PVC (Persistent Volume Claim): O PVC é como um "pedido" para obter armazenamento. Quando cria um PVC, é necessário especificar as necessidades do armazenamento, como o tamanho e a classe de armazenamento. O Kubernetes usa essas informações para vincular automaticamente o PVC a um PV compatível. 

#PV (Persistent Volume): O PV é um recurso de armazenamento, que pode ser alocado de várias fontes (disco, NFS, Google Cloud Storage, etc.). E neste PV você definie o que se pede em um PVC.
```

# O StorageClass é uma maneira de definir diferentes tipos de armazenamento disponíveis no cluster. Ele especifica as características de armazenamento, como tipo de disco, desempenho (ex: SSD ou HDD), ou outras opções específicas do provedor de nuvem (no seu caso, Google Cloud).

```bash
#StorageClass: Define o tipo de armazenamento (HDD, SSD, etc.) e como o volume é provisionado.
```

