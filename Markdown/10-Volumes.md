# :floppy_disk: Volumes

Tendo vista a volatilidade dos conteiners, na arquitetura do kubernetes,há várias abstrações para o armazenamento de informações. Tais como:

- [:floppy_disk: Volumes](#floppydisk-volumes)
  - [Volume Cloud](#volume-cloud)
  - [HostPath Volume](#hostpath-volume)
  - [EmptyDir Volume](#emptydir-volume)
  - [Persistent Volume](#persistent-volume)

## Volume Cloud

Armazenamento em nuvem. Os maiores players da área são, Amazon, Google e Microsoft.

## HostPath Volume

Utiliza o armazenamento local do nó. De forma bem simples, trata-se de uma especie de compartilhamento entre a máquina onde os pods são criados e o próprio pod. Dessa forma cria-se uma afinidade entre o pod e o nó, uma vez que se esse pod for destruído e recriado em um segundo nó seus dados permaneceram no primeiro nó.

Essa afinidade por ser provida por meio dos [labels](./08-Create-Labels-Service.md#:label:-Labels). Fazendo com que determinados pods apenas sejam criados em um nó especifico.

## EmptyDir Volume

Uma mistura de volatilidade com persistencia. Resumidamente, fica em pé enquanto o pod está em pé. Nesse tipo, caso algum dos containers do pod venha a falhar seus dados não serão perdidos, isso porque estão a salvos enquanto o pod estiver funcionando. Se o pod vier a falhar (leia-sê _restartar_), os dados serão perdidos.

Esse tipo de armazenamento é indicado para cache e coisas temporárias e não deve ser usado para dados sensíveis, a fim de evitar problemas com suas perdas.

## Persistent Volume

Por fim os volumes persistentes, provávelmente os mais completos em requisitos, dispensando afinidade e realmente fazendo a persistência, tudo de forma "local".

Esse tipo de armazenamento funciona em duas etapas:

1. PV - Persistent Volume (Disponibilização)
2. PVC - Persistent Volume Claim (Consumo)

Antes de poder ser utilizado/consumido esse armazenamento deve ser disponibilizado. Com eles é tudo ou nada, se tivermos dois PVs o primeiro com 100GB e o segundo com 50GB. Então um PVC requisitando 75GB, esse PVC será alocado no primeiro. Posteriormente temos um segundo PVC requisitando 100GB, esse não será atendido pois não há um PV com espaço suficiente, visto que um PVC só pode estar "linkado" com um único PV.

[Artigo sobre Volume Persistente Local](10.1-Persistent-Volume-Local.md)

**Declaração de um EmptyDir**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: exemplo-volume
  labels:
    name: exemplo-volume
spec:
  containers:
    - name: exemplo-volume
      image: "khaosdoctor/simple-node-api"
      env:
        - name: PORT
          value: "8080"
      resources:
        limits:
          memory: "128Mi"
          cpu: "500m"
      ports:
        - containerPort: 8080
      volumeMounts:
        - mountPath: /usr/src/app
          name: nome-do-volume
volumes:
  - name: nome-do-volume
    emptyDir: {}
```

**[Arquivo com as declarações usadas](../yml's/10-Volume.yml)**

