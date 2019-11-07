# :door: Como expor o Pod para fora do cluster

Até o momento os pods e seus containers apenas são visíveis dentro do cluster. Para expor os pods para a "internet" utilizaremos uma abstração de rede, os ***[Services](#buildingconstruction-services)***. No entanto há uma preparação para utilização de tal, os ***[Labels](#label-labels)***.

## :building_construction: Services

Sabemos que contêineres são extremamente voláteis/efémeros, portanto não temos como atribuir IPs para cada Pod, nesse momento entra em ação os Services.

O ***Services*** é a camada que garante a orquestração da atribuição de ips, internamente, para os Pod. Desta forma, para expormos uma determinada aplicação, na verdade estamos expondo o Service equivalente aos Pods que ela acessa.

## :label: Labels

Agora que sabemos o que os Services podem acessar vários Pods, para expor o Service precisamos definir quais Pods ele acessa.

Essa declaração de acesso é feita através dos ***Labels***, que é a única maneira de agrupar Pods. Dessa forma, quando os containers que pertencem aos Pods precisarem ser destruídos, ao retornarem, obterão as mesmas configurações de acesso ao Service designado pelo Label, ou seja, acessarão o mesmo Service de seus antecedentes.

### Exemplo:

```yml
apiVersion: v1
kind: Pod
metadata:
  name: simple-node-api-pod
  labels:
    app: simple-node-api-label
spec:
  containers:
  - name: simple-node-api-container
    image: josecarlosnf/simple-node-api
    env:
      - name: PORT
        value: "8080"
    ports:
      - containerPort: 8080
```

Adição da Label "app" com valor "simple-node-api-label" ao Pod "simple-node-api-pod".

## :couch_and_lamp: Conclusão
O Label nada mais é que uma etiqueta, para agrupamento de Pods. Podem ser utilizadas de diversas formas, desde o acesso a um Service ou mesmo como flag seletora para o comando ```kubectl```, como exemplificado a seguir.

```
kubectl get pod -l app=simple-node-api-label
```