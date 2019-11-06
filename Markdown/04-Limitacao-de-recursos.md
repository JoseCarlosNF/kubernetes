# :stop_sign: Limitação de recursos de um Pod.

Não temos poder computacional ilimitado, por isso devemos, até mesmo por questões de segurança e custo financeiro, limitar a quantidade de recursos que as aplicações poderão utilizar.

- Exemplo de Limitação de Recursos:

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: nome-do-pod
spec:
  containers:
  - name: nome-do-container
    image: imagem-do-container
    resources:
      requests:
        cpu: 100m
        memory: 128M
      limits:
        cpu: 250m
        memory: 256M
    ports:
    - containerPort: 27017
```

No caso em questão, o Pod está sendo instruído a operar com 10% da CPU e 128MB de mémoria, e limitado a usar no máximo 25% da CPU e 256MB de mémoria.