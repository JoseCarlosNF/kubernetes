# :key: Secrets

As secrets são elemento fundamental para a segurança da aplicação.
Esse workload é responsável por fazer com que os dados sensíveis, como senhas de banco de dados e artefatos similares, não fiquem expostos aos que tem acesso ao cluster, trazendo mais segurança para a aplicação.

**Criação de uma Secret**

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: nome-do-secret
type: Opaque
data:
  username: dXN1YXJpb3NlZ3Vyb3BhcmFhcGxpY2FjYW8K
  password: c2VuaGFzZWd1cmFwYXJhYXBsaWNhY2FvCg==
```

Ao consultarmos as secrets disponíveis pelo comando `kubectl get secrets`, veremos que temos outra além da secret que criamos. Isso porque, o próprio cluster durante sua criação precisa de elementos secrets para armazenar tokens, certificados e outros artefatos.

```shell
$ kubectl get secrets
NAME                  TYPE                                  DATA   AGE
default-token-jxtgw   kubernetes.io/service-account-token   3      5d18h
nome-do-secret        Opaque
```

## Utilização de secrets

Para utilizar as secrets temos pelo menos duas formas:
  - passando-as como **variáveis da ambiente**;
  - ou montando-as como **volumes**.

Aproveitaremos a secret anteriormente criada para exemplificar os casos.

### Variável de Ambiente

Tenha em mente que ao utilizar variáveis de ambiente, elas são integradas ao sistema(Pod) no momento de sua criação. Ou seja, são é possível atualiza-las em momento de execução.

**Secret como Variável de Ambiente**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-com-secret-env
spec:
  containers:
    - name: container-com-secret
      image: josecarlosnf/simple-node-api
      env:
        - name: SECRET_USERNAME  # Variável de ambiente
          valueFrom:
            secretKeyRef:  # Referencia entre nome do secret e a chave que deve ser acessada
              name: nome-do-secret 
              key: username # chave que deve ser acessada.
        - name: SECRET_PASSWORD
          valueFrom:
            secretKeyRef:
              name: nome-do-secret
              key: password 
```

Acessamos o pod com o comando `kubectl exec -it pod-com-secret-env sh`.

```shell
# echo $SECRET_USERNAME
usuarioseguroparaaplicacao

# echo $SECRET_PASSWORD
senhaseguraparaaplicacao
```

Dessa forma podemos passar variáveis de ambiente que serão usadas, por exemplo, para o usuário e senha do banco de dados, situação muito comum com a utilização de containers.

### Volume

Por sua vez, ao utilizarmos as secrets como volumes montados podemos atualiza-los em momento de execução, haja vista a característica dos volumes.

**Secret montado como Volume**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-com-secret-volume
spec:
  containers:
    - name: container-com-secret
      image: josecarlosnf/simple-node-api
      volumeMounts:
        - name: volume-com-secret
          mountPath: /mnt # Ponto de Montagem da Secret
          readOnly: true
  volumes:
    - name: volume-com-secret # Declaração de volume
      secret: { secretName: nome-do-secret } # tipo do volume: nome da secret
```

Ao acessar o pod com o comando `kubectl exec -it pod-com-secret-volume sh` poderemos conferir os secrets montados.

```shell
# ls -l /mnt

total 0
lrwxrwxrwx 1 root root 15 Jan 20 12:32 password -> ..data/password
lrwxrwxrwx 1 root root 15 Jan 20 12:32 username -> ..data/username
```

Ao conferir seus conteúdos veremos nossas credências.

```shell
# cat /mnt/username && cat /mnt/password

senhaseguraparaaplicacao
usuarioseguroparaaplicacao
```

## Conclusão
Com as secrets impulsionamos o controle de versão para a infraestrutura, uma vez que podemos publica-la sem preocupações com a exposição de credências. Visto que estão em um local diferente, e quando necessárias podem ser consumidas ou criadas sem impactos no código da infraestrutura em si.

**[Arquivo com as declarações usadas](../yml's/11.Secrets.yml)**

