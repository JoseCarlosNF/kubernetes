#  :boxing_glove: Pod vs. Deployment
Diferenças entre a criação de um Pod e um Deployment.
---

No seguinte exemplo podemos visualizar as diferenças entre a criação de um *Pod* e o *Deployment* de uma aplicação, principalmente em relação:

- High Availability
- ReplicaSet

Criação de um **Pod**
---

```
kubectl create -f [02-pod.(json ou yml)]
```

Criação de um **Deployment**, por linha de comando
---

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
