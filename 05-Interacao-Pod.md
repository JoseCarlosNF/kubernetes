# :keyboard: Como interagir com os Pods

A interação com os pods, ou seja, com os containers que estão rodando dentro dos pods, pode ser feita através de um comando semelhante ao utilizado, para a mesma tarefa, no Docker.

```
kubectl exec -it [nome-do-pod] [comando]
```

*Obs*: O container deve aceitar o comando.