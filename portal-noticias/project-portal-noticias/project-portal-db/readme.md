# para saber o login do banco de dados, faça o seguinte
## 1 - entre no pod com o comando: 
```Bash
kubectl exec -it db-noticias -- bash
```
##  2 - Dentro do pod do mysql, execute o seguinte comando: 

```Bash
mysql -u root -p
```

## 3 - pressione enter e digite a senha criada.
## 4 - apos digitar a senha, execute os seguintes comandos;

```Bash
 show databases; 
 use empresa-teste; 
 show tables
 select * from usuario; 
```
## 5 - após executar o select, vai aparecer login e senha, algo parecido
```Bash
+-----------+-------+-------+
| idusuario | login | senha |
+-----------+-------+-------+
|         1 | xxxxx | xxxx  |
+-----------+-------+-------+
```

## aplicando os services e configmap
```Bash
kubectl apply -f db-configmap.yml
kubectl apply -f svc-db-noticias.yml
```
