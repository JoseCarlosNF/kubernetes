# ✈️ Tipos de Ingress

- **Single Service Ingress**: Utilizado em casos onde apenas um serviço deve ser exposto.

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: single-svc-ingress
spec:
  backend:
    serviceName: testsvc
    servicePort: 80
```

- **Fan-out**: Tem com base a requisição HTTP e permite que vários serviços sejam acessados por um único IP. Em um exemplo simples, podemos definir que um site será acessado por `www.site.com/home` e seu gerenciador de banco de dados, phpmyadmin, por `www.site.com/phpmyadmin`.

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: simple-fanout-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: site.com
    http:
      paths:
      - path: /home
        backend:
          serviceName: site
          servicePort: 80
      - path: /phpmyadmin
        backend:
          serviceName: phpmyadmin
          servicePort: 8080
```

- **Virtual Hosting**: Um mesmo endereço IP é responsável por atender requisições de variados domínios. De maneira que, com a chegada da requisição HTTP `site1.com` ira direcionar para o service `site1`, de maneira análoga a requisição `site2.com` direciona o acesso ao service `site2`.

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: virtual-host-ingress
spec:
  rules:
  - host: site1.com
    http:
      paths:
      - backend:
          serviceName: site1
          servicePort: 80
  - host: site2.com
    http:
      paths:
      - backend:
          serviceName: site2
          servicePort: 80
```
