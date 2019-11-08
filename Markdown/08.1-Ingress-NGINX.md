# :earth_americas: Ingress NGINX

O **nginx-ingress-controller** cria uma rede interna acessível de qualquer Pod dentro do Cluster.

## Testar acesso ao service

```
kubectl describe service [nome-servico]
```

Algo como isso será retornado:
```
Name:              simple-node-api-service
Namespace:         default
Labels:            <none>
Annotations:       <none>
Selector:          app=simple-node-api-label
Type:              ClusterIP
IP:                10.110.98.231
Port:              <unset>  8085/TCP
TargetPort:        porta-api/TCP
Endpoints:         172.17.0.5:8080
Session Affinity:  None
Events:            <none>
```

Utilize o IP e a Porta, para acessar.

```
curl -w "\n" 10.110.98.231:8085
```

## DNS Interno

Internamente temos um serviço de DNS que atende da seguinte forma:

```
<nome-do-workload>.<namespace>.<tipo>.cluster.local
```

Exemplo:

```
http://simple-node-api-service.default.svc.cluster.local:8085
```
