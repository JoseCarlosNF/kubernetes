#  :factory: Criação de images para containers

Em um ambiente real de utilização de containers, não utilizaremos apenas images do ***Docker Hub***. Assim, teremos que buildar, isto é, construir nossas próprias images.

## :books: Containers Register

As images ficam armazenadas em algum lugar certo? Dentro da arquitetura de funcionamento de containers com Docker esse lugar é chamado de ***Containers Registers***, uma espécie de repositório onde são armazenadas as images.

No contexto de Containers Registers temos algumas opções como:

- Docker Hub: Publico online
- Azure Container Registry: Privado online
- Privado localmente

## :camera: Criando uma imagem

No exemplo a seguir criaremos a imagem de um site usando apache como servidor web e alocando o o diretório onde encontra-se os arquivos na máquina local.

### :file_folder: Dockerfile

```Dockerfile
FROM httpd
COPY [diretorio-local] /usr/local/apache2/htdocs
```
Com essas duas linhas construímos uma imagem que disponibiliza um site de forma simples, sem nenhum tipo de autenticação ou configuração mais avançada.

> A imagem criada está no Docker Hub e pode ser baixada.