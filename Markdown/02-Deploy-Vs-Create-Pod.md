#  :boxing_glove: Pod vs. Deployment
Diferenças entre a criação de um Pod e um Deployment.
---

No seguinte exemplo podemos visualizar as diferenças entre a criação de um *Pod* e o *Deployment* de uma aplicação, principalmente em relação:

- High Availability
- ReplicaSet

## `kubectl create`

O comando `kubectl` é contextualizado, isso significa qua podemos criar todos os workloads com ele, apenas passando o arquivo declarativo:

```shell
kubectl create -f [nome-do-arquivo]
```

Podemos até mesmo criar todos os workloads declarados em vários arquivos em um diretório.

```shell
kubectl create -f [diretório]
```

## Declaração de um Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nome-do-pod
spec:
  containers:
    - name: nome-do-container
      image: mongo:latest
      ports:
        - containerPort: 27017
```

**Crianção de Pod por linha de comando***

```
kubectl create pod [nome-do-pod] --image=[imagem-do-container]
```

## Declaração de um Deployment

```yaml
apiVersion: v1
kind: Deployment
metadata:
  name: nome-do-deploy
spec:
  replicas: 1 # Numero de replicas
  selector:
    matchLabel:
      chave: valor  # Labels que serão aceitas no deployment
  template: # Configuração referente ao Pod (template do pod)
    metadata:
      labels:
        chave: valor
    spec:
      containers:
        - name: nome-do-container
          image: mongo:latest
          ports:
            - containerPort: 27017
```

**Crianção de Deployment por linha de comando***

```
kubectl create deployment [nome-do-deployment] --image=[imagem-do-container]
```

## :clipboard: Considerações
A grande diferença dá-se no momento em que tentamos apagar os pods:

```
kubectl delete pod [nome-do-pod]
```
- Por Deployment:

    Quando o pod é criado através do ciclo de Deployment, existe todo uma série de etapas, que serão abordadas mais adiante, que garantem a alta disponibilidade da aplicação. De modo que, quando um Pod é apagado outro é instantaneamente criado para substituí-lo.

- Por linha de comando, diretamente:

    Quando um Pod é criado diretamente não passa pelas etapas que garantem sua alta disponibilidade. Logo, ao ser apagado, nenhum contêiner será levantado para substituí-lo.

**[Arquivo com as declarações usadas](../yml's/02-Criacao-Pod.yml)**
