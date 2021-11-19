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

Essa falha ocorre pois para o funcionamento básico do chroot é necessário que exista pelo menos o /bin/bash e os diretórios /lib e /lib64 para guardar as libs necessárias para o funcionamento do bash

Primeiramente vamos criar um diretório /bin
```
mkdir bin
```

Em seguida vamos copiar o /bin/bash da nossa máquina para a pasta bin que criamos no passo anterior
```
cp /bin/bash bin/
```

Agora vamos verificar as bibliotecas necessárias para o funcionamento do /bin/bash
```
ldd /bin/bash

linux-vdso.so.1 (0x00007ffe7c5e8000)
libtinfo.so.6 => /lib/x86_64-linux-gnu/libtinfo.so.6 (0x00007f32ac104000)
libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f32ac0fe000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f32abf0c000)
/lib64/ld-linux-x86-64.so.2 (0x00007f32ac26a000)
```

Conforme a saída acima vamos copiar as bibliotecas necessárias para nosso diretório container

Primeiro vamos criar dois diretório lib e lib64
```
mkdir lib lib64
```

Em seguida vamos copiar as libs para os respctivos diretórios que criamos
```
cp /lib/x86_64-linux-gnu/libtinfo.so.6 ./lib/
cp /lib/x86_64-linux-gnu/libdl.so.2 ./lib/
cp /lib/x86_64-linux-gnu/libc.so.6 ./lib/
cp /lib64/ld-linux-x86-64.so.2 ./lib64/
```

Agora ao executar novamento o comando chroot ele irá funcionar
```
sudo chroot . 

bash-5.0#
```

Porém ao tentar listar os arquivo com o comando ls veremos que o comando não vai funcionar
```
ls

bash: ls: command not found
```

Isso acontece pois subimos apenas o básico necessário, para que o comando ls funcione é necessário copiar as bibliotecas que ele utiliza para dentro do nosso container, da mesma forma como fizemos com o /bin/bash

Primeiramente vamos sair do chroot, utilizando o atalho Ctrl+d ou digitando exit no terminal

Copiar o ls para o nosso container
```
cp /bin/ls ./bin/
```

Em seguida vamos verificar as bibiotecas necessárias para o funcionamento do ls
```
ldd /bin/ls

linux-vdso.so.1 (0x00007fffe6b33000)
libselinux.so.1 => /lib/x86_64-linux-gnu/libselinux.so.1 (0x00007fc7ed901000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fc7ed70f000)
libpcre2-8.so.0 => /lib/x86_64-linux-gnu/libpcre2-8.so.0 (0x00007fc7ed67f000)
libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007fc7ed679000)
/lib64/ld-linux-x86-64.so.2 (0x00007fc7ed95b000)
libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007fc7ed656000)
```

Agora vamos copiar as bibliotecas para pasta lib ou lib64 respectivamente para as pastas do noosso container
```
cp /lib/x86_64-linux-gnu/libselinux.so.1 ./lib/
cp /lib/x86_64-linux-gnu/libc.so.6 ./lib/
cp /lib/x86_64-linux-gnu/libpcre2-8.so.0 ./lib/
cp /lib/x86_64-linux-gnu/libdl.so.2 ./lib/
cp /lib64/ld-linux-x86-64.so.2 ./lib64/
cp /lib/x86_64-linux-gnu/libpthread.so.0 ./lib/
```

Após esse passos ao executar o chroot o comando ls estará disponível
```bash
sudo chroot .

ls
bin  lib  lib64  teste
```

## namespaces

Existem outros tipos de namespaces para recursos diferentes, neste exemplo será utilizado o namespaces para isolamento de processos.
Isola os processos do container dos processos do host

## 
