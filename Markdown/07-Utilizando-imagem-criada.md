# :play_or_pause_button: Utilização de Imagem criada no Cluster

Tendo em vista que nossa imagem anteriormente criada já foi publicada, não há grandes complicações para executa-la em nosso cluster. Apenas precisamos de um arquivo contendo as declarações necessárias para tal.

- Arquivo declarativo para execução da imagem.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: api-pod
spec:
  containers:
    - name: simple-api
      image: [nome-da-imagem-criada] # e.g: josecarlosnf/simple-node-api
      env:
        - name: PORT
          value: "8080"
      ports:
      - containerPort: 8080
```

**[Arquivo com as declarações usadas](../yml's/07-Utilizando-imagem-criada.yml)**
