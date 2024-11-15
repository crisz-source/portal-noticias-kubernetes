## aplicando os services e configmap
```Bash
# Utilizando o banco de dados:
kubectl get svc

# saída esperada
NAME                   TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes             ClusterIP   10.96.0.1        <none>        443/TCP        93m
svc-db-noticias        ClusterIP   10.98.5.189      <none>        3306/TCP       11m
svc-portal-noticias    NodePort    10.104.188.26    <none>        80:30000/TCP   93m
svc-sistema-noticias   NodePort    10.100.169.151   <none>        80:30001/TCP   79m

# peguei o serviço do banco (svc-db-noticias) e adicionei a variável de ambiente no aqruivo configmap sistema-configmap.yml. Para saber a váriavel de ambiente, entrei no dentro do container do pod sistema-noticias
kubectl exec -it sistema-noticias -- bash

# dentro do container, execute:
ls 

# saída esperada
Dockerfile    bancodedados.php    excluir.php  index.php             noticias.php    php.ini              php.ini-production  theme       uploadClass.php
arquivos.php  docker-compose.yml  funcoes.php  inserir_noticias.php  notificacao.sh  php.ini-development  sair.php            upload.php  uploads

# execute
cat bancodedados.php 

# Saída esperada>
<?php

$host = getenv("HOST_DB");
$usuario = getenv("USER_DB");
$senha = getenv("PASS_DB");
$banco = getenv("DATABASE_DB");

?>

# aí está as variáveis de amibiente que o configmap necessita para fazer a conexao com o banco corretamente. 

```

