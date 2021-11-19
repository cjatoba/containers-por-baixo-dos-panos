# containers-por-baixo-dos-panos
Explicação do funcionamento de containers por baixo dos panos

## Funcionamento geral

Basicamento os containers utilizam conceitos e ferramentas nativas do Linux para isolar diretórios, idolar processos e 

## chroot

O chroot serve para isolar um diretório fazendo com que ele se comporte como um sistema operacional isolado

Para este exemplo vamos criar uma pasta com o nome container
```
mkdir container
```
Acessar a pasta container
```
cd container
```

Neste ponto ao tentar utilizar o chroot teremos o seguinte retorno:
```
sudo chroot .

chroot: failed to run command ‘/bin/bash’: No such file or directory
```

Para o funcionamento básico do chroot é necessário que existam pelo menos o /bin/bash  e dos diretórios /lib e /lib64 para guardar as libs necessárias para o funcionamento do bash

## namespaces

Existem outros tipos de namespaces para recursos diferentes, neste exemplo será utilizado o namespaces para isolamento de processos.
Isola os processos do container dos processos do host

## 
