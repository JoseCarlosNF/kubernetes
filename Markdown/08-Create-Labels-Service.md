# :door: Como expor o Pod para fora do cluster

Até o momento os pods e seus containers apenas são visíveis dentro do cluster. Para expor os pods para a "internet" utilizaremos um ***[Ingress](#fastforward-proxy-reverso)*** para apontar para um ***[Service](#buildingconstruction-services)***, que é camada de interação com os pods agrupados por um ***[Label](#label-labels)***.

## :construction: Services

Sabemos que contêineres são voláteis/efémeros, portanto não temos como atribuir IPs para cada Pod, nesse momento entra em ação os Services.

O ***Services*** é a camada que garante a orquestração da atribuição de ips, internamente, para os pods. Desta forma, ao expormos uma aplicação, na verdade estamos expondo o service correspondente aos pods dela.

## :label: Labels

Agora que sabemos o que os services podem acessar vários pods, para expor o service precisamos definir quais pods ele acessa.

Essa declaração de acesso é feita através dos ***Labels***, que é a única maneira de agrupar Pods. Dessa forma, quando os containers que pertencem aos Pods, com uma determinada label, precisarem ser destruídos, ao retornarem, serão acessados pelo mesmo Service de seus antecedentes, pois esses novos containers estão em um Pod com as Labels que garantem o acesso ao Service.

O ***Label*** nada mais é que uma etiqueta, para agrupamento de Pods. Podem ser utilizadas de diversas formas, desde o acesso a um Service ou mesmo como flag seletora para o comando `kubectl`, como exemplificado a seguir.

```
kubectl get pod -l app=simple-node-api-label
```

### :label:Definindo um Label

```yaml
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
        name: porta-api
```

Adição da Label "app" com valor "simple-node-api-label" ao Pod "simple-node-api-pod".

### :construction: Definindo um Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: simple-node-api-service
spec:
  selector:
    app: simple-node-api-label
  ports:
    - protocol: TCP
      port: 8085
      targetPort: porta-api
```

O Service "simple-node-api-service" é definido para escutar a porta 8085 e enviar as requisições para a porta nomeada como "porta-api", dos Pods com a Label "app: simple-node-api-label".

# :flight_arrival: Ingress
É por meio dele que as aplicações, que estão rodando no cluster, são acessadas do lado de fora, ele é responsável por apontar as rotas para os Services, que apontam para os pods que contenham as labels correspondentes do Service, tudo isso a partir de uma requisição http. Existem várias alternativas para realizar esse trabalho, uma das mais famosas é o servidor web [NGINX](https://www.nginx.com/).

Por se tratar-se de uma arquitetura comum vamos utilizar o **[nginx-ingress-controller](https://github.com/kubernetes/ingress-nginx/)**, que basicamente trata-se de um conjunto de arquivos de manifesto, similar ao usado para criar pods e services, anteriormente.

**[Arquivo com as declarações usadas](../yml's/08-Create-Labels-Service.yml)**
