---
title: "Primeiros comandos do Docker"
description: "Aprenda a criar, iniciar, listar e acessar containers usando os principais comandos do Docker."
date: 2026-07-01
draft: false

categories:
  - docker
  - linux

tags:
  - docker
  - containers
  - docker run
  - docker ps
  - docker start
  - docker stop
  - docker attach
---

esse é a continuação do artigo "Guia iniciante no docker".

Depois de entendermos os conceitos básicos do Docker, como o que é uma imagem e o que é um contêiner, vamos explorar alguns dos comandos mais usados para criar e executar contêineres.

## criando um container

Agora vamos criar um contêiner baseado na imagem do Ubuntu.

No terminal, execute:

```bash
docker run ubuntu
```

O comando `run` cria e executa um container usando a imagem informada, Como a imagem ubuntu ainda não existia no computador, o Docker faz o download automaticamente do Docker Hub antes de criar o container.

Como o contêiner não tinha nenhuma tarefa para executar, ele foi encerrado logo após ser criado.

Agora vamos criar outro container em modo interativo e fazer com que ele execute o Bash.

```bash
docker run -it ubuntu bash
```

depois que o container abrir e dermos o comando `ls` veremos os diretórios do ubuntu que está rodando isolado dentro do container.

Agora é possível usar esse ubuntu normalmente, atualizar(`apt update && apt upgrade`) e instalar alguns programas de terminal para testar exemplo: `apt install nano`,
saia com o comando `exit`.

o que aconteceu aqui:

- run = executa/cria um container.

- -i = (interactive): mantém a entrada padrão (STDIN) aberta, permitindo que você digite comandos dentro do container.

- -t = (TTY): cria um terminal virtual, fazendo com que o Bash funcione como se estivesse sendo executado em um terminal real.

- bash =  O Bash é um interpretador de comandos (shell). Neste caso ele está sendo passado como o processo que o Ubuntu deverá executar quando o contêiner iniciar.

passar `-it` juntos é a forma mais comum quando queremos "entrar" no container para usá-lo manualmente.

note que o comando `run` sempre cria um container novo, Isso quer dizer que a cada execução do comando `docker run ubuntu` cria um novo container.

Em outras palavras, um novo Ubuntu "nasce" a partir da imagem sempre que esse comando é executado.

## Iniciando um container

para acessar um container existente precisamos dizer qual é o nome dele, acontece que, quando usamos o comando `docker run ubuntu` por exemplo o docker cria um nome aleatório para ele.

para descobrir qual o nome do container podemos listar todos os containers existentes no computador com o comando:

```bash
docker ps -a
```

 com isso veremos uma lista com informações sobre os contêineres, como CONTAINER ID, IMAGE, COMMAND, STATUS e NAMES.

 ![Alt text](/public/images/posts/artigo_docker01.png)

Também podemos dar um nome ao container no momento da criação:

```bash
docker run -it --name meuContainer ubuntu
```

Note que o container inicia com um shell do Ubuntu porque essa é a configuração padrão dessa imagem. Outras imagens podem iniciar executando programas completamente diferentes.

depois de sairmos do container com `exit` podemos iniciar ele usando o nome que demos para ele com o comando `start`.

```bash
docker start meuContainer
```

porém com o comando `start` apenas iniciamos o container, se dermos um `docker ps -a` novamente veremos que o seu STATUS está `up`, o que quer dizer que ele está rodando em segundo plano, isso é útil em alguns casos.

para entrar em um container em execução usamos o comando `attach`:

```bash
docker attach meuContainer
```

Como o processo principal desse container é o Bash, o docker attach conecta você diretamente ao terminal em execução.

Para sair do container sem encerrá-lo use a sequência Ctrl + P seguida de Ctrl + Q, Se usar o comando `exit` o Bash será encerrado e como ele é o processo principal do container o container também será finalizado.

para parar um container o comando é:

```bash
docker stop meuContainer
```

O comando start também possui a opção `-ai`, que permite iniciar o container e conectar-se ao seu terminal automaticamente, mas essa opção só funciona quando o processo principal do contêiner aceita interação, como o Bash:

```bash
docker start -ai meuContainer
```

- -a = attach

- -i = interactive

resumo dos comandos + extras:

- `docker run` =  cria um novo container, Se a imagem ainda não existir no computador, o Docker faz o download dela automaticamente antes de criar o container.

- `docker ps` = mostra todos os containers em execução.

- `docker ps -a` = mostra todos os containers mesmo os parados.

- `docker run --name <nome> <imagem>` = cria um container atribuindo um nome a ele.

- `docker start <nome_container>` = inicia um container parado.

- `docker attach <nome_container>` = entra ou se anexa a um container em execução.

- `docker start -ai <nome_container>` = inicia um container e se conecta ao seu terminal.

- `docker stop <nome_container>` = para um container em execução.

Neste artigo aprendemos a criar, listar, iniciar e acessar containers.

No próximo artigo veremos como remover containers e imagens, além de entender quando cada um deve ser apagado.
