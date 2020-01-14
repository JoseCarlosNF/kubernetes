# :floppy_disk: Volumes

Tendo vista a volatilidade dos conteiners, na arquitetura do kubernetes,há várias abstrações para o armazenamento de informações. Tais como:

1. [Volumes Cloud](#Volume-Cloud)
2. [HostPath Volume](#HostPath-Volume)
3. [EmptyDir Volume](#EmptyDir-Volume)
4. [Persistent Volume](#Persistent-Volume)

## Volume Cloud

Armazenamento em nuvem. Os maiores players da área são, Amazon, Google e Microsoft.

## HostPath Volume

Utiliza o armazenamento local do nó. De forma bem simples, trata-se de uma especie de compartilhamento entre a máquina onde os pods são criados e o próprio pod. Dessa forma cria-se uma afinidade entre o pod e o nó, uma vez que se esse pod for destruído e recriado em um segundo nó seus dados permaneceram no primeiro nó.

Essa afinidade por ser provida por meio dos [labels](08-Create-Labels-Service#:label:-Labels). Fazendo com que derterminados pods apenas sejam criados em um nó especifico.

## EmptyDir Volume

Uma mistura de volatilidade com persistencia. Resumidamente, fica em pé enquanto o pod está em pé. Nesse tipo, caso algun dos containers do pod venha a falhar seus dados não serão perdidos, isso porque estão a salvos enquanto o pod estiver funcionando. Se o pod vier a falhar (leia-sê *restartar*), os dados serão perdidos.

Esse tipo de armazenamento é indicado para cache e coisas temporarias e não deve ser usado para dados sensiveis, a fim de evitar problemas com suas perdas.

## Persistent Volume

Por fim os volumes persistentes, provávelmente os mais completos em requisitos, dispensando afinidade e realmente fazendo a persistencia, tudo de forma "local".

Esse tipo de armazenamento funciona em duas etapas:

1. PV - Persistent Volume (Disponibilização)
2. PVC - Persistent Volume Claim (Consumo)

Antes de poder ser utilizado/consumido esse armazenamento deve ser disponibilizado. Com eles é tudo ou nada, se tivermos dois PVs o primeiro com 100GB e o segundo com 50GB. Então um PVC requisitando 75GB, esse PVC será alocado no primeiro. Posteriormente temos um segundo PVC requisitando 100GB, esse não será atendido pois não há um PV com espaço suficiente, visto que um PVC só pode estar "linkado" com um único PV.